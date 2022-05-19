<template>
  <div>
    <h1>Hello world</h1>
    <v-row>
      <v-col cols="3" v-for="(item, i) in foodie" :key="i">
        <cardie :title="item.title" :img ="item.img" :price="item.price"></cardie>
      </v-col>
    </v-row>
    <v-dialog v-model="dialog" width="500">
      <v-card>
        <v-form>
          <v-container>
            <v-col cols="5" md="4">
              <v-row>
                <v-text-field
                  v-model="posts.tambah.title"
                  :rules="nameRules"
                  :counter="100"
                  label="title"
                  required
                ></v-text-field>
              </v-row>
              <v-row>
                <v-text-field
                  v-model="posts.tambah.img"
                  :rules="nameRules"
                  :counter="10000"
                  label="img"
                  required
                ></v-text-field>
              </v-row>
              <v-row>
                <v-text-field
                  v-model="posts.tambah.price"
                  :rules="nameRules"
                  :counter="90"
                  label="price"
                  required
                ></v-text-field>
              </v-row>
            </v-col>
          </v-container>
        </v-form>
        <v-divider>this dividing</v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="dialog = false"> Close </v-btn>
          <v-btn color="primary" text @click="prosesTambah"> Add</v-btn>
        </v-card-actions>
      </v-card>
      <template v-slot:activator="{ on, attrs }">
        <v-btn color="blue lighten-4" dark v-bind="attrs" v-on="on" fab large elevation="3">
          <v-icon large color = "blue">mdi-plus</v-icon>
        </v-btn>
      </template>
    </v-dialog>
  </div>
</template>

<script>
import axios from "axios";
import Cardie from "./_card.vue";

export default {
  name: "ThisIsfoodies",
  data: () => ({
    foodie: [],
    posts: {
      prosesTambah: false,
      tambah: {
        title: "",
        img: "",
        price: "",
      },
    },
    dialog: false,
  }),
  components: {
    Cardie,
  },
  created() {
    this.getData();
  },
  methods: {
    getData() {
      axios
        .get("http://localhost:3100/foodie")
        .then((response) => (this.foodie = response.data))
        .catch((err) => {
          console.log(err);
        });
    },
    async postData() {
      let payload = this.posts.tambah;
      axios.post("http://localhost:3100/foodie", payload).then(
        (response) => {
          console.log(response);
          this.posts.prosesTambah = false;
          this.getData();
          alert("Sukses Insert Data");
        },
        (error) => {
          console.log(error);
        }
      );
    },
  },
};
</script>

<style>
</style>
