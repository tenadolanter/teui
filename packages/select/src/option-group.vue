<template>
  <ul class="td-select-group__wrap" v-show="visible">
    <li class="td-select-group__title">{{ label }}</li>
    <li>
      <ul class="td-select-group">
        <slot></slot>
      </ul>
    </li>
  </ul>
</template>

<script type="text/babel">
import Emitter from "@tenado/ui/src/mixins/emitter";

export default {
  mixins: [Emitter],

  name: "TdOptionGroup",

  componentName: "TdOptionGroup",

  props: {
    label: String,
    disabled: {
      type: Boolean,
      default: false,
    },
  },

  data() {
    return {
      visible: true,
    };
  },

  watch: {
    disabled(val) {
      this.broadcast("TdOption", "handleGroupDisabled", val);
    },
  },

  methods: {
    queryChange() {
      this.visible =
        this.$children &&
        Array.isArray(this.$children) &&
        this.$children.some((option) => option.visible === true);
    },
  },

  created() {
    this.$on("queryChange", this.queryChange);
  },

  mounted() {
    if (this.disabled) {
      this.broadcast("TdOption", "handleGroupDisabled", this.disabled);
    }
  },
};
</script>
