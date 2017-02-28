TEST
<template>
  <div>
    <p>Layout</p>
    ==================================
    <div><ui-header></ui-header></div>
    ----------------------------------
    <div><ui-main></ui-main></div>
    ----------------------------------
    <div><ui-footer></ui-footer></div>
  </div>
</template>

<script>
const uiHeader = {
  template: `<header>
    <nav>Dis be a navbar</nav>
  </header>`,
};

const uiMain = {
  template: `<main>
    <div>Dis be some content</div>
  </main>`,
};

const uiFooter = {
  template: `<footer>
    <div>Dis be some footer</div>
  </footer>`,
};

export default {
  name: 'Layout',
  components: {
    uiHeader, uiMain, uiFooter,
  },
};
</script>
