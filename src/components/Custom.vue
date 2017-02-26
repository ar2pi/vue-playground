<template>
  <div class="custom">
    <h1 v-once>This value will not change: {{ msg | uppercase(true) }}</h1>
    <h1><input type="text" v-model="msg"></h1>
    <span>This value here be dynamic: {{ msg }}</span>
    <h3 :title="testTitle">{{ test }}</h3>
    <button class="btn" @click="testMethod">Click this</button>
    <button class="btn" @click="otherTestMethod">Show todos</button>
    <ul v-if="hideTodos">
      <todo-item v-for="item in todos" :todo="item"></todo-item>
    </ul>
    <div>{{ printDate }}</div>
    <div :class="{ mounted: isMounted }">{{ counter }}<button @click="destroyInstance">Destroy instance</button></div>
  </div>
</template>

<script>
export default {
  name: 'Test',
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
      isMounted: false,
      counter: 0,
    };
  },
  beforeCreate() {
    console.log('=== Before Create ===');
    console.log(this);
  },
  mounted() {
    console.log('=== Mounted ===');
    // kind of jQuery(document).ready
    this.isMounted = true;
    this.$interval = setInterval(() => {
      this.counter++;
    }, 1000);
  },
  destroyed() {
    console.log('=== Detroyed ===');
    clearInterval(this.$interval);
  },
  components: {
    todoItem: {
      props: ['todo'],
      template: '<li>{{ todo.text }}</li>',
    },
  },
  methods: {
    testMethod() {
      console.log('clicked that bitch');
      console.log(this);
      this.msg = this.msg.split('').reverse().join('');
    },
    otherTestMethod() {
      this.hideTodos = !this.hideTodos;
    },
    destroyInstance() {
      this.$destroy();
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
  margin: 0 10px;
}

a {
  color: #42b983;
}

.mounted {
  color: green;
}
</style>
