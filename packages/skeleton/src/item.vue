<template>
  <div :class="['td-skeleton__item', `td-skeleton__${variant}`]">
    <img-placeholder v-if="variant === 'image'" />
  </div>
</template>

<script>
import ImgPlaceholder from "./img-placeholder";

export default {
  name: "TdSkeletonItem",
  props: {
    variant: {
      type: String,
      default: "text",
    },
  },
  components: {
    [ImgPlaceholder.name]: ImgPlaceholder,
  },
};
</script>
