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

      <template #search="{attributes, events}">
        <input
          class="vs__search form-control"
          :required="required"
          v-bind="attributes"
          v-on="events"
        />
      </template>

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
      required: Boolean,
      items: Object,
      value: [Array, String]
    },
    data() {console.log(this.required)
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

<style>
  * {
    --vs-dropdown-bg: #fff;
  }

  .v-select {
    background-color: var(--vs-dropdown-bg);
  }

  .vs__search {
    margin: 0;
  }

  .vs__dropdown-toggle {
    padding: 0;
  }
</style>
