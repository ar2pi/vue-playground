<template>
<div>
  <h1>A web audio player example</h1>
  <img src="../assets/logo.png"></img>

  <div v-for="track in tracks" class="track"
    :class="{ active: activeAudio.element === track }"
    @click="toggleTrack(track, $event)"
    :data-file="track.file">{{ track.title }}</div>

  <!-- <audio src="static/audio/EUPHONIK  - UNE SAISON EN ENFER.mp3" controls></audio> -->
</div>
</template>

<script>
const tracks = [
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

function loadAudioElement(file) {
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

function playSound(context, source) {
  let newBuffer = context.createMediaElementSource(source);
  newBuffer.connect(context.destination);
  source.play();
  return newBuffer;
}

export default {
  data() {
    return {
      AudioContext: initAudioContext(),
      activeAudio: {
        element: null,
        _source: null,
        _buffer: null,
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
    toggleTrack(track, e) {
      if (this.AudioContext !== null && this.activeAudio.element !== track) {
        let self = this;
        if (self.activeAudio._buffer !== null) {
          self.activeAudio._buffer.disconnect();
        }
        loadAudioElement(e.target.dataset.file).then(function(el) {
          self.activeAudio = {
            element: track,
            _source: el,
            _buffer: playSound(self.AudioContext, el),
          };
        }, function(el) {
          throw el.error;
        });
      }
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
