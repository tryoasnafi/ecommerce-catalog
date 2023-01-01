<script lang="ts">
export default {
  props: {
    title: String,
    category: String,
    rate: {
      type: Number,
      default: 0,
    },
    description: String,
    price: Number,
    nextProduct: {
      type: Function,
      required: true,
    },
  },
  computed: {
    cssProps() {
      return {
        "--color-product":
          this.category == "men's clothing" ? "#002772" : "#720060",
      };
    },
  },
};
</script>
<template>
  <div class="product__info" :style="cssProps">
    <div class="product__head">
      <h1 class="product__title">{{ title }}</h1>
      <div class="product__sub-title">
        <p class="product__category">{{ category }}</p>
        <p class="product__rating">
          {{ rate }}/5
          <span
            v-for="n in 5"
            :key="n"
            :class="{
              icon: true,
              'icon-circle': n > rate,
              'icon-circle-fill': n < rate,
            }"
          ></span>
        </p>
      </div>
    </div>
    <hr />
    <div class="product__body">
      <p class="product__description">
        {{ description }}
      </p>
    </div>
    <hr />
    <div class="product__footer">
      <p class="product__price">${{ price }}</p>
      <div class="product__actions">
        <button class="product__button button__buy-now">Buy now</button>
        <button
          class="product__button button__next-product"
          @click="nextProduct()"
        >
          Next product
        </button>
      </div>
    </div>
  </div>
</template>
<style scoped>
.product__info {
  padding: 1rem;
  height: 100%;
}

.product__title {
  font-size: 1.7rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
  color: var(--color-product);
}

.product__sub-title {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 8px;
  padding-bottom: 8px;
}

.product__body {
  padding-top: 16px;
  padding-bottom: 16px;
  height: 200px;
  overflow-y: auto;
}

.product__rate-icon {
  fill: var(--color-product);
  color: var(--color-product);
}

.product__footer {
  padding-top: 16px;
  padding-bottom: 16px;
}

.product__price {
  font-size: 1.5rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
  color: var(--color-product);
}

.product__actions {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0.75rem;
}

.product__button {
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 0.25rem;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  width: 100%;
}

.button__buy-now {
  background-color: var(--color-product);
  color: var(--c-white);
}

.button__buy-now:hover {
  background-color: var(--color-product);
  filter: brightness(0.9);
}

.button__next-product {
  background-color: var(--c-white);
  color: var(--color-product);
  box-shadow: inset 0 0 0 2px var(--color-product);
}

.button__next-product:hover {
  box-shadow: inset 0 0 0 3px var(--color-product);
}

.icon {
  display: inline-block;
  width: 16px;
  height: 16px;
  background-size: cover;
}

.icon-circle {
  background-image: url(../assets/images/rate.svg);
}
.icon-circle-fill {
  background-image: url(../assets/images/rate-fill.svg);
}
</style>
