<template>
  <main>
    <div class="container-fluid">
      <div class="row">
        <div class="col-sm-3"></div>
        <div class="col-sm-6">
          <div class="a-section">
            <div class="a-spacing-top-medium"></div>
            <h2 style="test-align: center">Add a new owner</h2>

            <form action>
              <!-- Owner Name-->
              <div class="a-spacing-top-medium">
                <label style="margin-bottom: 0px">Owner Name</label>
                <input
                  type="text"
                  class="a-input-text"
                  style="width: 100%"
                  v-model="name"
                  placeholder="Type the owner name here."
                  v-on:keyup.enter="onAddOwner"
                />
              </div>

              <!-- Owner Name-->
              <div class="a-spacing-top-medium">
                <label for style="margin-bottom: 0px">About Owner</label>
                <textarea
                  style="width: 100%"
                  placeholder="Provide details such as a product description"
                  v-model="about"
                ></textarea>
              </div>

              <!-- Owner Photo -->
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
                    <span class="a-button-text" @click="onAddOwner"
                      >Add owner</span
                    >
                  </span>
                </span>
              </div>

              <br />
              <!-- <ul class="list-group-item">
                <li v-for="owner in owners" :key="owner._id">
                  <ul>{{owner.name}}</ul>
                  <ul>{{owner.about}}</ul>
                  <ul>{{owner.photo}}</ul>
                </li>
              </ul>-->

              <ul class="list-group">
                <li
                  class="list-group-item"
                  v-for="owner in owners"
                  :key="owner._id"
                >
                  {{ owner.name }}
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
      // let response = await $axios.$get("http://localhost:3000/api/owners");

      let response = await $axios.$get(
        "https://api-amazon-clone.herokuapp.com/api/owners"
      );

      return {
        owners: response.owners
      };
    } catch (error) {
      console.log(error);
    }
  },

  data() {
    return {
      name: "",
      about: "",
      selectedFile: null,
      fileName: ""
    };
  },

  methods: {
    onFileSelected(event) {
      this.selectedFile = event.target.files[0];
      this.fileName = event.target.files[0].name;
    },

    async onAddOwner() {
      try {
        let data = new FormData();
        data.append("name", this.name);
        data.append("about", this.about);
        data.append("photo", this.selectedFile, this.selectedFile.name);

        // let response = this.$axios.$post(
        //   "http://localhost:3000/api/owners",
        //   data
        // );

        let response = this.$axios.$post(
          "https://api-amazon-clone.herokuapp.com/api/owners",
          data
        );

        // this.$router.push("/owner");
        // this.owners.push(data);
        this.owners.push(this.name);
      } catch (error) {
        console.log(error);
      }
    }
  }
};
</script>
