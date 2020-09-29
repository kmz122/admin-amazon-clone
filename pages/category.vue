<template>
  <main>
    <div class="container-fluid">
      <div class="row">
        <div class="col-sm-3"></div>
        <div class="col-sm-6">
          <div class="a-section">
            <div class="a-spacing-top-medium"></div>
            <h2 style="test-align: center">Add a new category</h2>

            <form action>
              <!-- Category type input-->

              <div class="a-spacing-top-medium">
                <label style="margin-bottom: 0px">Category Type</label>
                <input
                  type="text"
                  class="a-input-text"
                  style="width: 100%"
                  v-model="type"
                  placeholder="Input the category type here."
                  v-on:keyup.enter="onAddCategory"
                />
              </div>

              <!-- Button -->
              <hr />
              <div class="a-spacing-top-large">
                <span class="a-button-register">
                  <span class="a-button-inner">
                    <span class="a-button-text" @click="onAddCategory"
                      >Add category</span
                    >
                  </span>
                </span>
              </div>

              <br />
              <ul class="list-group">
                <li
                  class="list-group-item"
                  v-for="category in categories"
                  :key="category._id"
                >
                  {{ category.type }}
                </li>
              </ul>
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
      // let response = await $axios.$get("http://localhost:3000/api/categories");

      let response = await $axios.$get(
        "https://api-amazon-clone.herokuapp.com/api/categories"
      );

      return {
        categories: response.categories
      };
    } catch (error) {
      console.log(error);
    }
  },

  data() {
    return {
      type: ""
    };
  },

  methods: {
    async onAddCategory() {
      try {
        let data = { type: this.type };

        // let response = this.$axios.$post(
        //   "http://localhost:3000/api/categories",
        //   data
        // );

        let response = this.$axios.$post(
          "https://api-amazon-clone.herokuapp.com/api/categories",
          data
        );

        //   this.$router.push("/");
        this.categories.push(data);
      } catch (error) {
        console.log(error);
      }
    }
  }
};
</script>
