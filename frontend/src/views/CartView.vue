<script setup lang="ts">
import CartCard from "@/components/CartCard.vue";
import { useProductsStore } from "@/stores/productsStore";

const store = useProductsStore();
</script>

<template>
  <section class="heading">
    <h2>Cart</h2>
  </section>
  <section class="cart">
    <div class="cart__container cart-products">
      <ul class="cart__items">
        <li v-for="product in store.getAllProductsInCart" class="cart__item">
          <CartCard :product="product" />
        </li>
      </ul>
    </div>
    <div class="cart__container cart-summary">
      <h2 class="cart-summary__title">Order Summary</h2>
      <ul class="cart-summary__fees">
        <li class="cart-summary__fee cart-summary__subtotal">
          <span>Subtotal</span><span>$40.0</span>
        </li>
        <li class="cart-summary__fee cart-summary__shipping">
          <span>Shipping</span><span>$10.0</span>
        </li>
      </ul>
      <p class="cart-summary__total"><span>Total</span><span>$50.0</span></p>
      <button class="cart-summary__checkout">Checkout</button>
    </div>
  </section>
</template>

<style lang="scss">
.heading {
  max-width: 75rem;
  padding: 2rem;
  margin: auto;
  font-size: 2rem;
  text-transform: uppercase;
  @media screen and (max-width: 768px) {
    text-align: center;
  }
  @media screen and (max-width: 425px) {
    font-size: 1.2rem;
    text-align: center;
  }
}

.cart {
  margin: auto;
  padding: 2rem;
  max-width: 75rem;
  @media screen and (min-width: 1024px) {
    display: flex;
    gap: 2rem;
  }
  @media screen and (max-width: 425px) {
    padding: 0;
  }
  &__container {
    height: fit-content;
    background-color: #eee;
  }
  &-products {
    padding: 1rem;
    flex: 3;
  }
  &__items {
    display: flex;
    flex-direction: column;
    gap: 3rem;
    list-style: none;
  }
  &-summary {
    padding: 1rem;
    flex: 2;
    @media screen and (max-width: 1024px) {
      width: 100%;
    }
    &__title {
      margin-bottom: 2rem;
    }
    &__fees {
      display: flex;
      flex-direction: column;
      gap: 1rem;
      padding: 1rem 0;
      border-top: 1px solid #ccc;
      border-bottom: 1px solid #ccc;
      list-style: none;
    }
    &__fee {
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 0.9rem;
      font-weight: 300;
      text-transform: uppercase;
    }
    &__total {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 2rem;
      padding: 1rem 0;
      font-size: 1.2rem;
      font-weight: 600;
      text-transform: uppercase;
      border-bottom: 1px solid #ccc;
    }
    &__checkout {
      width: 100%;
      padding: 1rem 0;
      color: #fff;
      background-color: #222;
      &:active {
        background-color: #333;
      }
    }
  }
}
</style>