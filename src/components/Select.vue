<template>
  <div>
    <v-select
      v-model="selected"
      :multiple="multiple"
      :taggable="taggable"
      :options="options"
      :reduce="(option) => option.id">
    </v-select>
  </div>
</template>

<script>
  import vSelect from 'vue-select';
  import map from 'lodash/map';

  export default {
    name: 'ui-select',
    components: {
      vSelect,
    },
    props: {
      multiple: Boolean,
      taggable: Boolean,
      items: Object,
      value: [Array, String]
    },
    computed: {
      options: function () {
        return map(JSON.parse(this.items), (label, id) => {
          return {id, label};
        });
      },
      selected: function () {
        return JSON.parse(this.value);
      }
    }
  }
</script>

<style src="vue-select/dist/vue-select.css"></style>