<template>
  <div class="custom">
    <h1 v-once>{{ msg | uppercase(true) }}</h1>
    <h1>{{ msg }}</h1>
    <h3 v-bind:title="testTitle">{{ test }}</h3>
    <button class="btn" v-on:click="testMethod">Click Event</button>
    <button class="btn" v-on:click="otherTestMethod">Other Click Event</button>
    <ul v-if="hideTodos">
      <todo-item v-for="item in todos" v-bind:todo="item"></todo-item>
    </ul>
    <div><a :href="currentChapter" target="_blank">Current chapter in Vue.js guide</a></div>
    <div>{{ printDate }}</div>
  </div>
</template>

<script>
import Vue from 'vue';

Vue.component('todo-item', {
  // The todo-item component now accepts a
  // "prop", which is like a custom attribute.
  // This prop is called todo.
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>',
});

export default {
  name: 'Test',
  methods: {
    testMethod() {
      console.log('clicked that bitch');
      console.log(this);
      this.msg = this.msg.split('').reverse().join('');
    },
    otherTestMethod() {
      this.hideTodos = !this.hideTodos;
    },
  },
  filters: {
    uppercase(...args) {
      if (!args[0]) return '';
      const testval = args[0].toString().toUpperCase();
      if (args[1]) return `!!! ${testval}`;
      return testval;
    },
  },
  computed: {
    printDate() {
      return (new Date()).toString();
    },
  },
  data() {
    return {
      msg: 'Custom component msg value',
      test: 'jaja',
      testTitle: 'this\'d be a title :3',
      todos: [
        { text: 'TO DO' },
        { text: 'or NOT TO DO' },
        { text: 'that is the question' },
      ],
      hideTodos: false,
      currentChapter: 'https://vuejs.org/v2/guide/computed.html#Computed-Setter',
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
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
