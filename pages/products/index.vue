<!-- /products -->

<template>
  <main>
    <div class="container-fluid">
      <div class="row">
        <div class="col-sm-3"></div>
        <div class="col-sm-6">
          <div class="a-section">
            <div class="a-spacing-top-medium"></div>
            <h2 style="test-align: center">Add a new product</h2>

            <form action>
              <!-- Category Dropdown -->
              <div class="a-spacing-medium">
                <label for style="margin-bottom: 0px">Category</label>
                <select name id class="a-select-option" v-model="categoryID">
                  <option
                    v-for="category in categories"
                    :value="category._id"
                    :key="category._id"
                    class="value"
                  >
                    {{ category.type }}
                  </option>
                </select>
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
                />
              </div>

              <!-- Description textarea-->
              <div class="a-spacing-top-medium">
                <label for style="margin-bottom: 0px">Description</label>
                <textarea
                  style="width: 100%"
                  placeholder="Provide details such as a product description"
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
                </div>
              </div>

              <!-- Button -->
              <hr />
              <div class="a-spacing-top-large">
                <span class="a-button-register">
                  <span class="a-button-inner">
                    <span class="a-button-text" @click="onAddProduct"
                      >Add product</span
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
  async asyncData({ $axios }) {
    try {
      // Testing for local dev
      // let categories = $axios.$get("http://localhost:3000/api/categories");
      // let owners = $axios.$get("http://localhost:3000/api/owners");

      let categories = $axios.$get("/api/categories");
      let owners = $axios.$get("/api/owners");

      const [catResponse, ownerResponse] = await Promise.all([
        categories,
        owners,
      ]);
      //   console.log(catResponse);
      // console.log("ownerResponse.owners: ", ownerResponse.owners);
      // console.log("catResponse.categories: ", catResponse.categories);

      return {
        categories: catResponse.categories,
        owners: ownerResponse.owners,
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
      console.log(this.selectedFile);
      this.fileName = event.target.files[0].name;
    },

    async onAddProduct() {
      let data = new FormData();
      data.append("title", this.title);
      data.append("description", this.description);
      data.append("price", this.price);
      data.append("stockQuantity", this.stockQuantity);
      data.append("categoryID", this.categoryID);
      data.append("ownerID", this.ownerID);
      data.append("photo", this.selectedFile, this.selectedFile.name);

      // let response = await this.$axios.$post(
      //   "http://localhost:3000/api/products",
      //   data
      // );

      let response = await this.$axios.$post("/api/products", data);

      //api-amazon-clone.herokuapp.com/

      if (response.success) {
        console.log("result: ", response);
        // console.log("ownerID", this.ownerID);
        // console.log("categoryID", this.categoryID);
      }

      this.$router.push("/");
    },
  },
};
</script>
