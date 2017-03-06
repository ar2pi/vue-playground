<template>
<div>
  <h1>A web audio player example</h1>
  <img src="../assets/logo.png"></img>

  <div v-for="track in tracks" class="track"
    :class="{ active: activeAudio.$element === track }"
    @click="toggleTrack(track)">{{ track.title }}</div>

  <!-- <audio src="static/audio/EUPHONIK  - UNE SAISON EN ENFER.mp3" controls></audio> -->
</div>
</template>

<script>
const tracks = [
  {
    file: 'looperman.mp3',
    title: 'TEST SOUND',
  },
  {
    file: 'track1.mp3',
    title: 'EUPHONIK  - UNE SAISON EN ENFER',
  },
  {
    file: 'track2.mp3',
    title: 'EUPHONIK - YANKEE',
  },
  {
    file: 'track3.mp3',
    title: 'EUPHONIK - LOIN DE VOUS',
  },
  {
    file: 'track4.mp3',
    title: 'EUPHONIK - VICE ET TRAHISONS (VERSION ORIGINAL)',
  },
  {
    file: 'track5.mp3',
    title: 'EUPHONIK - ÉLISE',
  },
];

function initAudioContext() {
  let response = null;
  try {
    response = new (window.AudioContext || window.webkitAudioContext)();
  } catch (e) {
    throw new Error('The Web Audio API is unavailable');
  }
  return response;
}

function loadAudioFile(file) {
  const url = `static/audio/${file}`;
  return new Promise(function(resolve, reject) {
    let audio = new Audio();
    audio.addEventListener('canplay', function() {
      resolve(audio);
    });
    audio.addEventListener('error', reject);
    audio.src = url;
  });
}

function playAudio(context, el) {
  let source = context.createMediaElementSource(el);
  let analyser = context.createAnalyser();
  source.connect(analyser).connect(context.destination);
  el.play();
  return source;
}

export default {
  data() {
    return {
      AudioContext: initAudioContext(),
      activeAudio: {
        $element: null,
        object: null,
        _source: null,
      },
      tracks,
    };
  },
  beforeCreate() {
    console.log('=== Before Create ===');
    // console.log(this);
  },
  mounted() {
    console.log('=== Mounted ===');
    // kind of jQuery(document).ready
  },
  components: {
    // todoItem: {
    //   props: ['todo'],
    //   template: '<li>{{ todo.text }}</li>',
    // },
  },
  methods: {
    toggleTrack(track) {
      if (this.AudioContext !== null) {
        if (this.activeAudio.$element !== track) {
          this.stopCurrentTrack();
          this.loadTrack(track);
        } else if (this.AudioContext.state === 'running') {
          // manage via AudioContext to keep track of state
          this.AudioContext.suspend();
          // self.activeAudio.object.pause();
        } else if (this.AudioContext.state === 'suspended') {
          this.AudioContext.resume();
          // self.activeAudio.object.play();
        }
      }
    },
    loadTrack(track) {
      if (this.activeAudio.$element === null || track.file !== this.activeAudio.$element.file) {
        let self = this;
        // load and play new track
        loadAudioFile(track.file).then(function(el) {
          self.switchTrack(track, el);
        }, function(el) {
          throw el.error;
        });
      } else if (track !== this.activeAudio.$element) {
        // play new track, no need to reload file
        self.switchTrack(track, this.activeAudio.object);
      } else {
        // replay track
        playAudio(this.AudioContext, this.activeAudio.object);
      }
    },
    switchTrack(track, el) {
      this.activeAudio = {
        $element: track,                            // vue element
        object: el,                                 // native audio object
        _source: playAudio(this.AudioContext, el),  // media element audio source node
      };
      let self = this;
      el.addEventListener('ended', function() {
        self.nextTrack();
      });
    },
    stopCurrentTrack() {
      if (this.activeAudio.object !== null) {
        if (!this.activeAudio.object.paused) {
          this.activeAudio.object.pause();
        }
        this.activeAudio.object.currentTime = 0;
      }
      if (this.activeAudio._source !== null) {
        this.activeAudio._source.disconnect();
      }
    },
    nextTrack() {
      this.loadTrack(this.tracks[(this.tracks.indexOf(this.activeAudio.$element) + 1) % this.tracks.length]);
    },
    previousTrack() {
      this.loadTrack(this.tracks[((this.tracks.indexOf(this.activeAudio.$element) - 1) + this.tracks.length) % this.tracks.length]);
    },
  },
  filters: {
    // uppercase(...args) {
    //   if (!args[0]) return '';
    //   const testval = args[0].toString().toUpperCase();
    //   if (args[1]) return `!!! ${testval}`;
    //   return testval;
    // },
  },
  computed: {
    // printDate() {
    //   return (new Date()).toString();
    // },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .track {
    padding: 10px;
    background-color: #eee;
    border-bottom: 1px solid #ccc;
    cursor: pointer;
  }
  .track:hover,
  .track:active,
  .track.active {
    background-color: green;
  }
</style>
