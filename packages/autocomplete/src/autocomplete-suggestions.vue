<template>
  <transition name="td-zoom-in-top" @after-leave="doDestroy">
    <div
      v-show="showPopper"
      class="td-autocomplete-suggestion td-popper"
      :class="{ 'is-loading': !parent.hideLoading && parent.loading }"
      :style="{ width: dropdownWidth }"
      role="region"
    >
      <td-scrollbar
        tag="ul"
        wrap-class="td-autocomplete-suggestion__wrap"
        view-class="td-autocomplete-suggestion__list"
      >
        <li v-if="!parent.hideLoading && parent.loading">
          <i class="td-icon-loading"></i>
        </li>
        <slot v-else> </slot>
      </td-scrollbar>
    </div>
  </transition>
</template>
<script>
import Popper from "@tenado/ui/src/utils/vue-popper";
import Emitter from "@tenado/ui/src/mixins/emitter";
import TdScrollbar from "@tenado/ui/packages/scrollbar";

export default {
  name: "TdAutocompleteSuggestions",
  components: { TdScrollbar },
  mixins: [Popper, Emitter],

  componentName: "TdAutocompleteSuggestions",

  data() {
    return {
      parent: this.$parent,
      dropdownWidth: "",
    };
  },

  props: {
    options: {
      default() {
        return {
          gpuAcceleration: false,
        };
      },
    },
    id: String,
  },

  methods: {
    select(item) {
      this.dispatch("TdAutocomplete", "item-click", item);
    },
  },

  updated() {
    this.$nextTick((_) => {
      this.popperJS && this.updatePopper();
    });
  },

  mounted() {
    this.$parent.popperElm = this.popperElm = this.$el;
    this.referenceElm =
      this.$parent.$refs.input.$refs.input ||
      this.$parent.$refs.input.$refs.textarea;
    this.referenceList = this.$el.querySelector(
      ".td-autocomplete-suggestion__list"
    );
    this.referenceList.setAttribute("role", "listbox");
    this.referenceList.setAttribute("id", this.id);
  },

  created() {
    this.$on("visible", (val, inputWidth) => {
      this.dropdownWidth = inputWidth + "px";
      this.showPopper = val;
    });
  },
};
</script>
