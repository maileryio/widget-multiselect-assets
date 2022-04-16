<template>
  <div>
    <v-select taggable multiple :options="['Canada', 'United States']"></v-select>
  </div>
</template>

<script>
  import vSelect from 'vue-select'

  Vue.component('v-select', vSelect)

  export default {
    name: 'ui-select',
    components: {
      vSelect,
    }
  }
</script>

<style src="vue-select/dist/vue-select.css"></style>