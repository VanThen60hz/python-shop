<template>
  <div class="home">
    <section class="hero is-medium is-dark mb-6">
      <div class="hero-body has-text-centered">
        <p class="title mb-6">Welcome to DEVFEST</p>
        <p class="subtitle">The best shopping store online</p>
      </div>
    </section>

    <div class="columns is-multiline">
      <div class="column is-12">
        <h2 class="is-size-2 has-text-centered">All products</h2>
      </div>

      <ProductBox
        v-for="product in allProducts"
        :key="product.id"
        :product="product"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import ProductBox from "@/components/ProductBox";

export default {
  name: "Test",
  data() {
    return {
      allProducts: [],
    };
  },
  components: {
    ProductBox,
  },
  mounted() {
    this.getLatestProducts();
    document.title = "Home | DEVFEST";
  },
  methods: {
    async getLatestProducts() {
      this.$store.commit("setIsLoading", true);

      try {
        const response = await axios.get("/api/v1/get-all-products/");
        this.allProducts = response.data;
      } catch (error) {
        console.error(error);
      }

      this.$store.commit("setIsLoading", false);
    },
  },
};
</script>
