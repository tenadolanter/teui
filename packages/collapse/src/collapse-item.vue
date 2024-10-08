<template>
  <div
    class="td-collapse-item"
    :class="{ 'is-active': isActive, 'is-disabled': disabled }"
  >
    <div
      role="tab"
      :aria-expanded="isActive"
      :aria-controls="`td-collapse-content-${id}`"
      :aria-describedby="`td-collapse-content-${id}`"
    >
      <div
        class="td-collapse-item__header"
        @click="handleHeaderClick"
        role="button"
        :id="`td-collapse-head-${id}`"
        :tabindex="disabled ? undefined : 0"
        @keyup.space.enter.stop="handleEnterClick"
        :class="{
          focusing: focusing,
          'is-active': isActive,
        }"
        @focus="handleFocus"
        @blur="focusing = false"
      >
        <slot name="title">{{ title }}</slot>
        <i
          class="td-collapse-item__arrow td-icon-arrow-right"
          :class="{ 'is-active': isActive }"
        >
        </i>
      </div>
    </div>
    <td-collapse-transition>
      <div
        class="td-collapse-item__wrap"
        v-show="isActive"
        role="tabpanel"
        :aria-hidden="!isActive"
        :aria-labelledby="`td-collapse-head-${id}`"
        :id="`td-collapse-content-${id}`"
      >
        <div class="td-collapse-item__content">
          <slot></slot>
        </div>
      </div>
    </td-collapse-transition>
  </div>
</template>
<script>
import TdCollapseTransition from "@tenado/ui/src/transitions/collapse-transition";
import Emitter from "@tenado/ui/src/mixins/emitter";
import { generateId } from "@tenado/ui/src/utils/util";

export default {
  name: "TdCollapseItem",

  componentName: "TdCollapseItem",

  mixins: [Emitter],

  components: { TdCollapseTransition },

  data() {
    return {
      contentWrapStyle: {
        height: "auto",
        display: "block",
      },
      contentHeight: 0,
      focusing: false,
      isClick: false,
      id: generateId(),
    };
  },

  inject: ["collapse"],

  props: {
    title: String,
    name: {
      type: [String, Number],
      default() {
        return this._uid;
      },
    },
    disabled: Boolean,
  },

  computed: {
    isActive() {
      return this.collapse.activeNames.indexOf(this.name) > -1;
    },
  },

  methods: {
    handleFocus() {
      setTimeout(() => {
        if (!this.isClick) {
          this.focusing = true;
        } else {
          this.isClick = false;
        }
      }, 50);
    },
    handleHeaderClick() {
      if (this.disabled) return;
      this.dispatch("TdCollapse", "item-click", this);
      this.focusing = false;
      this.isClick = true;
    },
    handleEnterClick() {
      this.dispatch("TdCollapse", "item-click", this);
    },
  },
};
</script>
