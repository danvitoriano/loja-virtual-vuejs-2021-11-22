<template>
  <div>
    <base-header>
      <b-navbar variant="dark" type="dark">
        <b-navbar-brand href="#">Carrinho: {{ cart.length }}</b-navbar-brand>
      </b-navbar>
    </base-header>
    <b-container fluid>
      <product :premium="premium" @add-to-cart="updateCart"></product>
    </b-container>
  </div>
</template>

<script>
import Product from "./components/Product.vue";

export default {
  name: "App",
  data() {
    return {
      premium: true,
      cart: [],
    };
  },
  methods: {
    updateCart(id) {
      this.cart.push(id);
    },
  },
  components: {
    product: Product,
  },
};
</script>
