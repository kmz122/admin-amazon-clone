<template>
  <main>
    <div class="a-spacing-large"></div>

    <div class="continer-fluid browsing-history">
      <div class="row">
        <div class="col-sm-8 col-8">
          <h1 class="a-size-large a-spacing-none a-text-normal">
            All products
          </h1>
          <div class="a-spacing-large"></div>
          <!-- Button -->
          <nuxt-link to="/products" class="a-button-buy-again"
            >Add a new product</nuxt-link
          >
          <nuxt-link to="/category" class="a-button-history margin-right-10"
            >Add a new category</nuxt-link
          >
          <nuxt-link to="/owner" class="a-button-history margin-right-10"
            >Add a new owner</nuxt-link
          >
        </div>
      </div>
    </div>

    <div class="a-spacing-large"></div>

    <!-- Listing page -->
    <div class="container-fluid browsing-history">
      <div class="row">
        <div
          v-for="(product, index) in products"
          :key="product._id"
          class="col-xs-2 col-lg-2 col-md-3 col-sm-6 br bb"
        >
          <div class="history-box">
            <div class="text-center">
              <!-- Product Image -->
              <a href="#" class="a-link-normal">
                <img :src="product.photo" alt="#" class="img-fluid" />
              </a>
              <!-- Product Title -->
              <div class="a-spacing-top-base asin-title">
                <span class="a-text-normal">
                  <div class="p13n-sc-truncated">{{ product.title }}</div>
                </span>
              </div>

              <!-- Product Rating -->
              <div class="a-row">
                <a href>
                  <i class="fas fa-star"></i>
                  <i class="fas fa-star"></i>
                  <i class="fas fa-star"></i>
                  <i class="fas fa-star"></i>
                  <i class="fas fa-star"></i>
                </a>

                <span class="a-letter-space"></span>
                <span class="a-color-tertiary a-size-small asin-review"
                  >(1782)</span
                >
              </div>

              <!-- Product Price -->
              <div class="a-row">
                <span class="a-size-base a-color-price">
                  <span class="p13n-sc-price">${{ product.price }}</span>
                </span>
              </div>

              <div class="a-spacing-large"></div>

              <!-- Product Button -->
              <div class="a-row">
                <nuxt-link
                  :to="`/products/${product._id}`"
                  class="a-button-history margin-right-10"
                  >Update</nuxt-link
                >
                <a
                  href
                  @click="onDeleteProduct(product._id, index)"
                  class="a-button-history margin-right-10"
                  >Delete</a
                >
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- <div v-for="product in products" :key="product._id" class="show_products">{{product._id}}</div> -->
    </div>
  </main>
</template>

<script>
export default {
  // asyncData is fetching data before nuxt finished loading on the browser.
  // It is good for SEO because the dat will be loaded first.
  async asyncData({ $axios }) {
    try {
      // let response = await $axios.$get("http://localhost:3000/api/products");
      let response = await $axios.$get("/api/products");

      // console.log(response);
      // console.log("This is only the products array from the response");
      // console.log(response.products);

      return {
        products: response.products
      };
    } catch (err) {
      console.log(err);
    }
  },

  methods: {
    async onDeleteProduct(id, index) {
      try {
        // let response = await this.$axios.$delete(
        //   `http://localhost:3000/api/products/${id}`
        // );

        let response = await this.$axios.$delete(`/api/products/${id}`);

        if (response.status) {
          this.products.splice(index, 1);
          this.$nuxt.refresh();
        }
      } catch (error) {
        console.log(error);
      }
    }
  }
};
</script>

<style></style>
