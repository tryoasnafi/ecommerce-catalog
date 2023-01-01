<script lang="ts">
export default {
  props: {
    nextProduct: {
      type: Function,
      required: true,
    },
  },
};
</script>
<template>
  <section class="unavailable">
    <h1>This product is unavailable to show</h1>
    <button class="button__next-product" @click="nextProduct()">
      Next product
    </button>
  </section>
</template>
<style scoped>
.unavailable {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
}

.button__next-product {
  margin-top: 0.7rem;
  padding: 0.625rem 2rem;
  background-color: var(--c-white);
  color: var(--c-blue-dark);
  box-shadow: inset 0 0 0 2px var(--c-blue-dark);
  cursor: pointer;
}

.button__next-product:hover {
  box-shadow: inset 0 0 0 3px var(--c-blue-dark);
}
</style>
