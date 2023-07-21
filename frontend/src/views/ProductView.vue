<script setup lang="ts">
import { computed } from "vue";
import { storeToRefs } from "pinia";
import { useProductsStore } from "@/stores/productsStore";
import type { Product, FavoriteProduct } from "@/types/productTypes";

const props = defineProps<{ id: string }>();

const store = useProductsStore();

const { getProductById } = storeToRefs(store);

const product: Product | undefined = getProductById.value(Number(props.id));

const favoriteProducts = computed<FavoriteProduct[]>(
  () => store.getAllFavoriteProducts
);

const isFavorite = computed<FavoriteProduct | undefined>(() =>
  favoriteProducts.value.find((p) => p.productId === product?.productId)
);

const favoriteHandler = () => {
  if (!product) return;

  if (!isFavorite.value) {
    store.addNewFavoriteProduct(product);
  } else {
    store.deleteFavoriteProduct(product);
  }
};

const getFormattedPrice = (number: number): string => {
  const isNumber = !isNaN(number);

  if (!isNumber) {
    return "Price not available";
  } else {
    const formattedPrice = new Intl.NumberFormat("en-CA", {
      style: "currency",
      currency: "CAD",
    }).format(Math.abs(number));
    return formattedPrice;
  }
};

const addToCart = () => {
  if (product) store.addNewProductToCart(product);
};
</script>

<template>
  <section v-if="product" class="product">
    <div class="product__column product__column--left">
      <img class="product__image" :src="product.productImg" alt="" />
    </div>
    <div class="product__column product__column--right">
      <div class="product__info">
        <div class="product__header">
          <h1 class="product__title">{{ product.productTitle }}</h1>
          <img
            v-if="isFavorite"
            class="product__like"
            @click="favoriteHandler"
            src="@/assets/heart-red.svg"
            alt="Add to favorites"
          />
          <img
            v-else
            class="product__like"
            @click="favoriteHandler"
            src="@/assets/heart.svg"
            alt="Add to favorites"
          />
        </div>
        <div class="product__price">
          {{ getFormattedPrice(product.productPrice) }}
        </div>
        <button @click="addToCart" class="product__button">Add to bag</button>
      </div>
    </div>
  </section>
</template>

<style lang="scss">
.product {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(26rem, 1fr));
  justify-items: stretch;
  gap: 2rem;
  width: 100%;
  height: 100%;
  padding: 2rem;
  &__column {
    min-width: 20px;
    &--left {
      text-align: center;
    }
  }
  &__image {
    width: 100%;
    height: 50rem;
    object-fit: cover;
  }
  &__info {
    position: sticky;
    top: 5rem;
    padding: 1rem 2rem 0 2rem;
  }
  &__header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1rem;
  }
  &__title {
    font-size: 1rem;
    font-weight: 600;
  }
  &__like {
    width: 2.4rem;
    cursor: pointer;
  }
  &__price {
    font-size: 1.25rem;
    font-weight: 700;
    margin-bottom: 2rem;
  }
  &__button {
    width: 100%;
    padding: 0.8rem 0;
    color: #fff;
    background-color: #222;
    font-weight: 600;
    transition: background-color 0.1s ease-in-out;
    &:active {
      background-color: #888;
    }
  }
}

// Responsive styles
@media screen and (max-width: 768px) {
  .product {
    grid-template-columns: repeat(auto-fit, minmax(24rem, 1fr));
  }
}
@media screen and (max-width: 475px) {
  .product {
    grid-template-columns: repeat(auto-fit, minmax(14rem, 1fr));
  }
}
</style>