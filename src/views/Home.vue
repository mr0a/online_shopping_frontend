<template>
  <div class="home">
    <section class="hero is-medium is-primary mb-6">
      <div class="hero-body has-text-centered">
        <p style="color: black" class="title mb-6">
          Welcome
          <br />
          to
          <br />
          Fresh Farm Foods
        </p>
        <p class="subtitle">Freshly from Farm to your home</p>
      </div>
    </section>

    <div class="columns is-multiline">
      <div class="column is-12">
        <h2 class="is-size-2 has-text-centered">Latest Products</h2>
      </div>
      <ProductBox
        v-for="product in latestProducts"
        v-bind:key="product.id"
        v-bind:product="product"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import ProductBox from "@/components/ProductBox";

export default {
  name: "Home",
  data() {
    return {
      latestProducts: [],
    };
  },
  components: {
    ProductBox
  },
  mounted() {
    this.getLatestProducts();
    document.title = "Home | Farm Fresh";
  },
  methods: {
    getLatestProducts() {
      axios
        .get("/api/v1/latest-products/")
        .then((response) => {
          this.latestProducts = response.data;
        })
        .catch(console.log);
    },
  },
};
</script>
