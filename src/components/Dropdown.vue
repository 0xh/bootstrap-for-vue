<template>
<div :class="[`drop${type}`, (show ? 'show' :  '')]">
  <slot></slot>

  <div class="dropdown-menu" v-if="items.length">
    <slot name="items">
      <div v-for="(item, index) in items" class="dropdown-item" :class="{ active: active === index }"
           v-bind="{ active: active === index, selected: selected.indexOf(item) > -1, item}"
           :is="component" :key="item" role="button"
           @click.native.prevent="$emit('select', item)"
           @click.prevent="$emit('select', item)">{{ item[itemKey] }}
      </div>
    </slot>
  </div>
  <div class="dropdown-menu" v-else>
    <slot name="empty"></slot>
  </div>
</div>
</template>

<script lang="babel">
import inherit from '../mixins/inheritComponents'

export default {
  props: {
    show: {
      type: Boolean,
      required: true
    },

    active: {},

    selected: {
      type: Array,
      default: () => []
    },

    type: {
      validator (type) {
        return ['up', 'down'].indexOf(type) > -1
      },
      default: 'down'
    },

    items: {
      type: Array,
      default: () => []
    },

    itemKey: {
      type: String,
      default: 'name'
    },

    component: {
      type: String,
      default: 'div'
    }
  },

  mixins: [inherit]
}

</script>

<style lang="scss">
.dropdown-menu {
  max-height: 300px;
  overflow-y: auto;

  .dropdown-item {
    cursor: pointer;

    &[selected] {
      font-weight: bolder;

      &:before {
        content: "✓";
        display: inline-block;
        margin-left: -17.5px;
        margin-right: 3.5px;
      }
    }
  }
}
</style>
