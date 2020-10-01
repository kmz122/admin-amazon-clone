<!-- /product/products_id_in_number -->

<template>
  <main>
    <div class="container-fluid">
      <div class="row">
        <div class="col-sm-3"></div>
        <div class="col-sm-6">
          <div class="a-section">
            <div class="a-spacing-top-medium"></div>
            <h2 style="test-align: center">Update {{ product.title }}</h2>

            <form action>
              <!-- Category Dropdown -->
              <div class="a-spacing-medium">
                <label for style="margin-bottom: 0px">Category</label>
                <!-- <select name id class="a-select-option" v-model="categoryID"> -->

                <template>
                  <div v-if="categoryID">
                    <select
                      name
                      id
                      class="a-select-option"
                      v-model="categoryID"
                    >
                      <option
                        v-for="cat in categories"
                        :value="cat._id"
                        :key="cat._id"
                        class="value"
                      >
                        {{ cat.type }}
                      </option>
                    </select>
                  </div>
                </template>
              </div>

              <!-- Owner Dropdown-->
              <div class="a-spacing-medium">
                <label for style="margin-bottom: 0px">Owner</label>
                <select name id class="a-select-option" v-model="ownerID">
                  <option
                    v-for="owner in owners"
                    :value="owner._id"
                    :key="owner._id"
                    class="value"
                  >
                    {{ owner.name }}
                  </option>
                </select>
              </div>

              <!-- Title input-->
              <div class="a-spacing-top-medium">
                <label style="margin-bottom: 0px">Title</label>
                <input
                  type="text"
                  class="a-input-text"
                  style="width: 100%"
                  v-model="title"
                  :placeholder="product.title"
                />
              </div>

              <!-- Price input-->
              <div class="a-spacing-top-medium">
                <label style="margin-bottom: 0px">Price</label>
                <input
                  type="text"
                  class="a-input-text"
                  style="width: 100%"
                  v-model="price"
                  :placeholder="product.price"
                />
              </div>

              <!-- StockQuantity input-->
              <div class="a-spacing-top-medium">
                <label style="margin-bottom: 0px">Stock Quantity</label>
                <input
                  type="text"
                  class="a-input-text"
                  style="width: 100%"
                  v-model="stockQuantity"
                  :placeholder="product.stockQuantity"
                />
              </div>

              <!-- Description textarea-->
              <div class="a-spacing-top-medium">
                <label for style="margin-bottom: 0px">Description</label>
                <textarea
                  style="width: 100%"
                  :placeholder="product.description"
                  v-model="description"
                ></textarea>
              </div>

              <!-- Photo file-->
              <div class="a-spacing-top-medium">
                <label for style="margin-bottom: 0px">Photo</label>
                <div class="a-row a-spacing-top-medium">
                  <label class="choosefile-button">
                    <i class="fal fa-plus"></i>
                    <input type="file" @change="onFileSelected" />
                    <p style="margin-top: -70px">{{ fileName }}</p>
                  </label>
                  <!-- <img :src="selectedFile" alt="your product photo" /> -->
                  <!-- <b-img :src="selectedFile" fluid alt="Product photo"></b-img> -->
                </div>
              </div>

              <!-- Button -->
              <hr />
              <div class="a-spacing-top-large">
                <span class="a-button-register">
                  <span class="a-button-inner">
                    <span class="a-button-text" @click="onUpdateProduct"
                      >Update product</span
                    >
                  </span>
                </span>
              </div>
            </form>
          </div>
        </div>
        <div class="col-sm-3"></div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  async asyncData({ $axios, params }) {
    try {
      // let categories = $axios.$get("http://localhost:3000/api/categories");
      // let owners = $axios.$get("http://localhost:3000/api/owners");
      // let product = $axios.$get(
      //   `http://localhost:3000/api/products/${params.id}`
      // );

      let categories = $axios.$get("/api/categories");
      let owners = $axios.$get("/api/owners");
      let product = $axios.$get(`/api/products/${params.id}`);

      const [catResponse, ownerResponse, productResponse] = await Promise.all([
        categories,
        owners,
        product,
      ]);
      //   console.log(catResponse);
      // console.log(("From Product Update page: ", productResponse));
      //   console.log(ownerResponse);

      return {
        categories: catResponse.categories,
        owners: ownerResponse.owners,
        product: productResponse.product,
        categoryID: productResponse.product.category._id,
        ownerID: productResponse.product.owner._id,
        title: productResponse.product.title,
        price: productResponse.product.price,
        stockQuantity: productResponse.product.stockQuantity,
        description: productResponse.product.description,
        selectedFile: productResponse.product.photo,
        selectedFileCheck: productResponse.product.photo,
        // fileName: String(productResponse.product.photo),
      };
    } catch (err) {
      console.log(err);
    }
  },

  data() {
    return {
      categoryID: null,
      ownerID: null,
      title: "",
      price: 0,
      stockQuantity: 0,
      description: "",
      selectedFile: null,
      fileName: "",
    };
  },

  methods: {
    onFileSelected(event) {
      this.selectedFile = event.target.files[0];
      console.log("event.target.files", event.target.files);
      this.fileName = event.target.files[0].name;
    },

    async onUpdateProduct() {
      let data = new FormData();
      data.append("title", this.title);
      data.append("description", this.description);
      data.append("price", this.price);
      data.append("stockQuantity", this.stockQuantity);
      data.append("categoryID", this.categoryID);
      data.append("ownerID", this.ownerID);

      // if (this.selectedFile !== this.selectedFileCheck) {
      data.append("photo", this.selectedFile, this.selectedFile.name);
      // } else {
      //   data.append("photo", "");
      // }
      // else data.append("photo", this.fileName);

      // let result = await this.$axios.$put(
      //   `http://localhost:3000/api/products/${this.$route.params.id}`,
      //   data
      // );

      await this.$axios.$put(`/api/products/${this.$route.params.id}`, data);

      this.$router.push("/");
    },
  },
};
</script>
