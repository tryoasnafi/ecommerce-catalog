<script lang="ts">
export default {
  props: {
    imageUrl: {
      type: String,
      required: true,
    },
    imageAlt: String,
  },
};
</script>
<template>
  <div class="product__image">
    <img :src="imageUrl" :alt="imageAlt" />
  </div>
</template>
<style scoped>
.product__image {
  padding: 1rem;
  overflow: hidden;
}
.product__image img {
  width: 100%;
  padding: 16px;
}
</style>
