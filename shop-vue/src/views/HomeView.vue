<template>
  <div class="home">
    <section class="hero is-medium mb-6">
      <div
        id="carouselExampleControls"
        class="carousel slide"
        data-bs-ride="carousel"
      >
        <div class="carousel-inner">
          <div class="carousel-item active">
            <img
              src="../assets/baner.jpg"
              style="height: 650px"
              class="d-block w-100"
              alt="..."
            />
          </div>
          <div class="carousel-item">
            <img
              src="../assets/banner2.jpg"
              style="height: 650px"
              class="d-block w-100"
              alt="..."
            />
          </div>
          <div class="carousel-item">
            <img
              src="../assets/baner3.jpg"
              style="height: 650px"
              class="d-block w-100"
              alt="..."
            />
          </div>
        </div>
        <button
          class="carousel-control-prev"
          type="button"
          data-bs-target="#carouselExampleControls"
          data-bs-slide="prev"
        >
          <span class="carousel-control-prev-icon" aria-hidden="true"></span>
          <span class="visually-hidden">Previous</span>
        </button>
        <button
          class="carousel-control-next"
          type="button"
          data-bs-target="#carouselExampleControls"
          data-bs-slide="next"
        >
          <span class="carousel-control-next-icon" aria-hidden="true"></span>
          <span class="visually-hidden">Next</span>
        </button>
      </div>
    </section>

    <div class="columns is-multiline">
      <div class="column is-12">
        <h2 class="is-size-2 has-text-centered">Latest products</h2>
      </div>

      <ProductBox
        v-for="product in latestProducts.results"
        :key="product.id"
        :product="product"
      />
    </div>

    <!-- Thêm nút Next và Previous -->
    <div class="pagination mt-6 has-text-centered">
      <button
        v-if="latestProducts.previous"
        @click="loadPage(latestProducts.previous)"
        class="button is-info mr-2"
      >
        Previous
      </button>
      <button
        v-if="latestProducts.next"
        @click="loadPage(latestProducts.next)"
        class="button is-info"
      >
        Next
      </button>
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
      latestProducts: {
        count: 0,
        next: null,
        previous: null,
        results: [],
      },
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
    async getLatestProducts(url = "/api/v1/latest-products/") {
      this.$store.commit("setIsLoading", true);

      await axios
        .get(url)
        .then((response) => {
          this.latestProducts = response.data;
        })
        .catch((error) => {
          console.log(error);
        });

      this.$store.commit("setIsLoading", false);
    },
    loadPage(url) {
      this.getLatestProducts(url);
    },
  },
};
</script>
