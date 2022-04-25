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
      :class="[className, focused ? 'focused' : '']"
      :multiple="multiple"
      :taggable="taggable"
      :clearable="clearable"
      :searchable="searchable"
      :deselectFromDropdown="deselectFromDropdown"
      :closeOnSelect="closeOnSelect"
      :options="options"
      :placeholder="placeholder"
      :reduce="(option) => option.id"
      :selectable="(option) => !!option.id"
      @open="() => focused = true"
      @close="() => focused = false">

      <template #search="{attributes, events}">
        <input
          class="vs__search"
          :required="required"
          v-bind="attributes"
          v-on="events"
        />
      </template>

      <template #open-indicator="{ attributes }">
        <span v-bind="attributes">
          <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAYAAADgdz34AAAABmJLR0QA/wD/AP+gvaeTAAAAbklEQVRIie2PMQqAMAxFXwfP6qJVPIjYzcOKdUkhQ0BaCoLkLS3k818CjuN8ztApYzIDl7xvmalFsAIZuIFozKPMMrC0CAJwKIkuGVX5KdkmLEm3ci1JSlLKU49yLSmXVG1es0EANvnvInKcP/AA784fpjlWwNQAAAAASUVORK5CYII="/>
        </span>
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
        }),
        focused: false
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
  .ui-select {
    .v-select {
      background-color: #fff;
      height: auto;

      &.form-control.focused {
        border-color: #80bdff;
        box-shadow: 0 0 0 0.2rem rgb(0 123 255 / 25%);

        &.is-valid {
          border-color: #28a745;
          box-shadow: 0 0 0 0.2rem rgb(40 167 69 / 25%);
        }

        &.is-invalid {
          border-color: #dc3545;
          box-shadow: 0 0 0 0.2rem rgb(220 53 69 / 25%);
        }
      }
    }

    .vs__search {
      margin: 0;
    }

    .vs__actions {
      padding: 0;
    }

    .vs__dropdown-toggle {
      border: none;
      padding: 0;
    }

    .vs__dropdown-menu {
      margin-top: 3px;
    }

    .vs__selected {
      margin: 2px 2px 0;
    }
  }
</style>
