<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h3><router-link :to="{ name: 'Custom' }">/custom</router-link></h3>
    <h3><router-link :to="{ name: 'Layout' }">/layout</router-link></h3>
    <h3><router-link :to="{ name: 'AudioPlayer' }">/audio-player</router-link></h3>
    <div><a :href="tuto" target="_blank">Vue.js guide</a></div>
    <div><a :href="doc" target="_blank">API reference</a></div>
    <div><a :href="examples" target="_blank">Vue examples</a></div>
    <h2>Other resources</h2>
      <div><a href="https://www.grafikart.fr/formations/vuejs" target="_blank">Grafikart</a></div>
    <h2>Essential Links</h2>
    <ul>
      <li><a href="https://vuejs.org" target="_blank">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org" target="_blank">Forum</a></li>
      <li><a href="https://gitter.im/vuejs/vue" target="_blank">Gitter Chat</a></li>
      <li><a href="https://twitter.com/vuejs" target="_blank">Twitter</a></li>
      <br>
      <li><a href="http://vuejs-templates.github.io/webpack/" target="_blank">Docs for This Template</a></li>
    </ul>
    <h2>Ecosystem</h2>
    <ul>
      <li><a href="http://router.vuejs.org/" target="_blank">vue-router</a></li>
      <li><a href="http://vuex.vuejs.org/" target="_blank">vuex</a></li>
      <li><a href="http://vue-loader.vuejs.org/" target="_blank">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank">awesome-vue</a></li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'test',
  data() {
    return {
      msg: 'Let\'s play',
      test: '/Custom',
      tuto: 'https://vuejs.org/v2/guide/',
      doc: 'https://vuejs.org/v2/api/',
      examples: 'http://vuejsexamples.com/',
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
