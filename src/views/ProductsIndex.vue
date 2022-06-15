<script>
var axios = require("axios");

export default {
  data: function () {
    return {
      products: [],
      nameFilter: "",
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("/products.json").then((response) => {
        console.log("All products", response.data);
        this.products = response.data;
      });
    },
    filterProducts: function () {
      return this.products.filter((product) => {
        return product.name.toLowerCase().includes(this.nameFilter.toLowerCase());
      });
    },
  },
};
</script>

<template>
  <h1>All products</h1>
  <div class="mb-3">
    Filter products by name:
    <input v-model="nameFilter" type="text" list="names" />
    <datalist id="names">
      <option v-for="product in products" v-bind:key="product.id">{{ product.name }}</option>
    </datalist>
  </div>
  <div class="row row-cols-1 row-cols-md-3 g-4">
    <TransitionGroup name="list">
      <div v-for="product in filterProducts()" v-bind:key="product.id" class="col">
        <div class="card">
          <img v-bind:src="product.image_url" class="card-img-top" alt="" />
          <div class="card-body">
            <h5 class="card-name">Name: {{ product.name }}</h5>
            <p class="card-text">Price: {{ product.price }}</p>
            <a class="btn btn-primary" v-bind:href="`/products/${product.id}`">Show more info</a>
          </div>
        </div>
      </div>
    </TransitionGroup>
  </div>
</template>

<style>
.card img {
  height: 300px;
  object-fit: cover;
}

.list-move, /* apply transition to moving elements */
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}

/* ensure leaving items are taken out of layout flow so that moving
   animations can be calculated correctly. */
.list-leave-active {
  position: absolute;
}
</style>
