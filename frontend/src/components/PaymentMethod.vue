<script setup lang="ts">
import { ref } from "vue";
import { useRoute } from "vue-router";

const route = useRoute();

const selectedPaymentMethod = ref("paypal");
</script>
<template>
  <div class="payment__container payment-method">
    <h3>Payment Method</h3>
    <div>
      <label for="paypalButton">
        <i
          v-if="
            route.name === 'placeorder' && selectedPaymentMethod === 'paypal'
          "
          class="fa-solid fa-circle"
        ></i>
        <i
          v-if="
            route.name === 'placeorder' && selectedPaymentMethod !== 'paypal'
          "
          class="fa-regular fa-circle"
        ></i>
        <i class="fa-brands fa-paypal"></i>
        <span>Paypal</span>
      </label>
      <input
        type="radio"
        id="paypalButton"
        value="paypal"
        v-model="selectedPaymentMethod"
      />
    </div>
  </div>
</template>
<style lang="scss">
.payment-method {
  color: #3f3f3f;
  label {
    display: flex;
    gap: 0.875rem;
    font-size: 1.25rem;
    align-items: center;
  }
  input {
    display: none;
  }
}
</style>
