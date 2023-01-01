<script lang="ts">
import { ref } from "vue";
import ProductImage from "./ProductImage.vue";
import ProductInfo from "./ProductInfo.vue";
import ProductLoading from "./ProductLoading.vue";
import ProductUnavailable from "./ProductUnavailable.vue";

type Product = {
  id: number;
  title: string;
  price: number;
  description: string;
  category: string;
  image: string;
  rating: {
    rate: number;
    count: number;
  };
};

const KEY_ID_PRODUCT = "last_viewed_product_id";

const loadIdProduct = (): number | null =>
  Number(localStorage.getItem(KEY_ID_PRODUCT));
const saveIdProduct = (id: number) =>
  localStorage.setItem(KEY_ID_PRODUCT, id.toString());

const state = {
  product: ref<Product | undefined>(undefined),
  idProduct: ref<number>(loadIdProduct() || 1),
  isLoading: ref<boolean>(false),
  error: ref(null),
};

const fetchProduct = async (id: number): Promise<Product | undefined> => {
  try {
    const response = await fetch(`https://fakestoreapi.com/products/${id}`);
    return response.json();
  } catch (err: any) {
    console.error(err.message);
  }
};

const backgroundVariant = (category: string) => {
  switch (category) {
    case "men's clothing":
      return "var(--color-background-men)";
    case "women's clothing":
      return "var(--color-background-women)";
    default:
      return "var(--color-background)";
  }
};

const incrementIdProduct = (id: number) => {
  return id < 20 ? id + 1 : 1;
};

const nextProduct = async () => {
  state.isLoading.value = true;
  state.error.value = null;
  saveIdProduct(state.idProduct.value);
  try {
    state.idProduct.value = incrementIdProduct(state.idProduct.value);
    const product = await fetchProduct(state.idProduct.value);
    state.product.value = product;
    document.body.style.background = backgroundVariant(
      state.product.value?.category || ""
    );
  } catch (err: any) {
    console.log("error");
    state.error.value = err.message;
  } finally {
    state.isLoading.value = false;
  }
};

const isUnavailable = (category: string): boolean =>
  category !== "men's clothing" && category !== "women's clothing";

export default {
  setup() {
    nextProduct();
    return {
      state,
      nextProduct,
      isUnavailable,
    };
  },
  components: { ProductImage, ProductInfo, ProductLoading, ProductUnavailable },
};
</script>

<template>
  <div v-if="state.error.value" class="error">
    {{ state }}
    Oops! Error encountered: {{ state.error }}
  </div>
  <div v-else-if="state.isLoading.value" class="product--loading">
    <ProductLoading />
  </div>
  <div
    v-else-if="isUnavailable(state.product.value?.category || '')"
    class="product--unavailable"
  >
    <ProductUnavailable :next-product="nextProduct" />
  </div>
  <div v-else class="product">
    <ProductImage
      :image-url="state.product.value?.image || '#'"
      :image-alt="state.product.value?.title"
    />
    <ProductInfo
      :title="state.product.value?.title"
      :category="state.product.value?.category"
      :rate="state.product.value?.rating.rate"
      :description="state.product.value?.description"
      :price="state.product.value?.price"
      :next-product="nextProduct"
    />
  </div>
</template>

<style scoped>
.product,
.product--loading,
.product--unavailable {
  width: 1034px;
  height: 580px;
  place-items: center;
  width: 1034px;
  height: 580px;
  margin: 0 auto;
  padding: 2.5rem;
  background-color: var(--c-white);
  border: 1px solid #ccc;
  border-radius: 0.625rem;
  box-shadow: 0 4px 4px rgba(0, 0, 0, 0.25);
}
.product {
  display: grid;
  grid-template-columns: 2fr 3fr;
}

.product--unavailable {
  background: url("../assets/images/sad-face.svg") no-repeat;
  background-size: cover;
  background-color: white;
}

.product--loading {
  display: flex;
  justify-content: center;
  align-content: center;
}
</style>
