<template>
  <div>
    <div v-if="selected && this.multiple">
      <input v-for="value in selected" type="hidden" :name="name + '[]'" :value="value" />
    </div>
    <div v-else-if="selected">
      <input type="hidden" :name="name" :value="selected" />
    </div>

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
      name: String,
      multiple: Boolean,
      taggable: Boolean,
      items: Object,
      value: [Array, String]
    },
    data() {
      return {
        options: map(JSON.parse(this.items), (label, id) => {
          return {id, label};
        }),
        selected: (() => {
          if (this.multiple && this.value) {
            return JSON.parse(this.value);
          }
          return this.value;
        })()
      }
    }
  }
</script>

<style src="vue-select/dist/vue-select.css"></style>
