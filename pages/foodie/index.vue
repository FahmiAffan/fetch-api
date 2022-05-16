<template>
  <div>
    <h1>Hello world</h1>
    <v-dialog v-model="dialog" width="500">
      <template v-slot:activator="{ on, attrs }">
        <v-btn color="red lighten-2" dark v-bind="attrs" v-on="on">
          Click Me
        </v-btn>
      </template>

      <v-card>
        <v-form>
          <v-container>
            <v-col cols="12" md="4">
              <v-row>
                <v-text-field
                  v-model="posts.tambah.title"
                  :rules="nameRules"
                  :counter="10"
                  label="title"
                  required
                ></v-text-field>
              </v-row>
              <v-row>
                <v-text-field
                  v-model="posts.tambah.img"
                  :rules="nameRules"
                  :counter="10"
                  label="img"
                  required
                ></v-text-field>
              </v-row>
              <v-row>
                <v-text-field
                  v-model="posts.tambah.img"
                  :counter="10"
                  label="Image"
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
          <v-btn color="primary" text @click="dialog = false"> Add</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-row>
      <v-col cols="3" v-for="(item, i) in foodie" :key="i">
        <cardie :title="item.title" :img="item.img"></cardie>
      </v-col>
    </v-row>
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