<template>
  <li
    class="td-dropdown-menu__item"
    :class="{
      'is-disabled': disabled,
      'td-dropdown-menu__item--divided': divided,
    }"
    @click="handleClick"
    :aria-disabled="disabled"
    :tabindex="disabled ? null : -1"
  >
    <i :class="icon" v-if="icon"></i>
    <slot></slot>
  </li>
</template>
<script>
import Emitter from "@tenado/ui/src/mixins/emitter";

export default {
  name: "TdDropdownItem",

  mixins: [Emitter],

  props: {
    command: {},
    disabled: Boolean,
    divided: Boolean,
    icon: String,
  },

  methods: {
    handleClick(e) {
      this.dispatch("TdDropdown", "menu-item-click", [this.command, this]);
    },
  },
};
</script>
