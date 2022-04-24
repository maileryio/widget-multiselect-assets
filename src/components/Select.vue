<template>
  <div class="ui-select">
    <div v-if="hasSelected && multiple">
      <input v-for="value in selected" type="hidden" :name="name + '[]'" :value="value" />
    </div>
    <div v-else-if="hasSelected">
      <input type="hidden" :name="name" :value="selected" />
    </div>

    <v-select
      v-model="selected"
      :class="className"
      :multiple="multiple"
      :taggable="taggable"
      :clearable="clearable"
      :searchable="searchable"
      :deselectFromDropdown="deselectFromDropdown"
      :closeOnSelect="closeOnSelect"
      :options="options"
      :placeholder="placeholder"
      :reduce="(option) => option.id"
      :selectable="(option) => !!option.id">

      <template #search="{attributes, events}">
        <input
          class="vs__search"
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
  import filter from 'lodash/filter';
  import map from 'lodash/map';

  export default {
    name: 'ui-select',
    components: {
      vSelect,
    },
    props: {
      name: String,
      multiple: {
      	type: Boolean,
      	default: false
      },
      taggable: {
      	type: Boolean,
      	default: false
      },
      searchable: {
      	type: Boolean,
      	default: true
      },
      clearable: {
      	type: Boolean,
      	default: true
      },
      deselectFromDropdown: {
          type: Boolean,
          default: false
      },
      closeOnSelect: {
      	type: Boolean,
      	default: true
      },
      required: {
      	type: Boolean,
      	default: false
      },
      items: Object,
      value: [Array, String],
      className: String,
      placeholder: String
    },
    data() {
      return {
        selected: (() => {
          const value = this.value ? JSON.parse(this.value) : null;

          if (!this.multiple && Array.isArray(value)) {
            return value[0];
          }
          return value;
        })(),
        options: map(JSON.parse(this.items), (label, id) => {
          return {id, label};
        })
      }
    },
    computed: {
      hasSelected () {
        return this.selected && this.selected.length > 0;
      },
      required () {
        return this.$props.required && this.selected && this.selected.length == 0;
      }
    }
  }
</script>

<style src="vue-select/dist/vue-select.css"></style>

<style lang="scss">
  * {
    --vs-dropdown-bg: #fff;
  }

  .ui-select {
    .v-select {
      background-color: var(--vs-dropdown-bg);
      height: auto;
    }

    .vs__search {
      margin: 0;
    }

    .vs__dropdown-toggle {
      border: none;
      padding: 0 0 2px;
    }

    .vs__selected {
      margin: 2px 2px 0;
    }
  }
</style>
