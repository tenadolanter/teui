<template>
  <span class="td-breadcrumb__item">
    <span
      :class="['td-breadcrumb__inner', to ? 'is-link' : '']"
      ref="link"
      role="link"
    >
      <slot></slot>
    </span>
    <i
      v-if="separatorClass"
      class="td-breadcrumb__separator"
      :class="separatorClass"
    ></i>
    <span v-else class="td-breadcrumb__separator" role="presentation">{{
      separator
    }}</span>
  </span>
</template>
<script>
export default {
  name: "TdBreadcrumbItem",
  props: {
    to: {},
    replace: Boolean,
  },
  data() {
    return {
      separator: "",
      separatorClass: "",
    };
  },

  inject: ["elBreadcrumb"],

  mounted() {
    this.separator = this.elBreadcrumb.separator;
    this.separatorClass = this.elBreadcrumb.separatorClass;
    const link = this.$refs.link;
    link.setAttribute("role", "link");
    link.addEventListener("click", (_) => {
      const { to, $router } = this;
      if (!to || !$router) return;
      this.replace ? $router.replace(to) : $router.push(to);
    });
  },
};
</script>
