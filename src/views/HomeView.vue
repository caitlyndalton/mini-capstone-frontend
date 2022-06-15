<script>
import axios from "axios";

export default {
  data: function () {
    return {
      products: [],
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("/products.json").then((response) => {
        console.log("products index", response.data);
        this.products = response.data;
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>All Products</h1>
    <div v-for="product in products" v-bind:key="product.id">
      <h2>{{ product.name }}</h2>
      <p>Price: {{ product.price }}</p>
      <p>Description: {{ product.description }}</p>
    </div>
  </div>
</template>

<style></style>
