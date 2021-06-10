<template>
  <div id="app">
    <mscombobox></mscombobox>
  </div>
</template>
<script>
import mscombobox from './components/comobobox.vue'

export default {
  name: 'app',
  components: {
    mscombobox
  }
}
</script>
<style lang="scss">
*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
@import url("./style/combobox.css");
</style>
