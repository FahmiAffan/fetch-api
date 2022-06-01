<template>
  <div>
    <v-card>
      <v-app-bar color="white" scroll-target="#scrolling-techniques-7">
        <div class="text-center">
          <v-menu offset-y>
            <template v-slot:activator="{ on, attrs }">
              <v-btn text v-bind="attrs" v-on="on">
                <v-icon>mdi-dots-vertical</v-icon>
              </v-btn>
            </template>
            <v-list>
              <v-list-item>
                <v-list-item-title>
                  <v-btn text>Fahmi</v-btn>
                </v-list-item-title>
              </v-list-item>
              <v-list-item>
                <v-list-item-title>
                  <v-btn text>ganteng</v-btn>
                </v-list-item-title>
              </v-list-item>
            </v-list>
          </v-menu>
        </div>

        <v-toolbar-title class="font-weight-bold">FOOD'IE</v-toolbar-title>

        <v-spacer></v-spacer>

        <v-btn icon>
          <v-icon>mdi-magnify</v-icon>
        </v-btn>

        <v-btn icon>
          <v-icon>mdi-dots-vertical</v-icon>
        </v-btn>
      </v-app-bar>
      <v-sheet id="scrolling-techniques-7"> </v-sheet>
    </v-card>
    <br />
    <div class="text-right">
      <v-menu offset-y>
        <template v-slot:activator="{ on, attrs }">
          <v-btn
            v-bind="attrs"
            v-on="on"
            class="mr-16 px-10"
            text
            elevation="4"
          >
            Sort BY
          </v-btn>
        </template>
        <v-list>
          <v-list-item>
            <v-list-item-title>
              <v-btn text> Most Popular </v-btn>
            </v-list-item-title>
          </v-list-item>
          <v-list-item>
            <v-list-item-title>
              <v-btn text> Highest Price </v-btn>
            </v-list-item-title>
          </v-list-item>
          <v-list-item>
            <v-list-item-title>
              <v-btn text> Lowest Price </v-btn>
            </v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>
    </div>
    <v-row>
      <v-col cols="3" v-for="(item, i) in foodie" :key="i">
        <cardie
          :title="item.title"
          :img="item.img"
          :price="item.price"
          :rating="item.rating"
          :emitting="item"
          @klikDelete="deleteData"
        ></cardie>
      </v-col>
    </v-row>
    <v-dialog v-model="dialog" width="500">
      <v-card>
        <v-form>
          <v-container>
            <v-row>
              <v-col cols="5" md="4">
                <v-text-field
                  v-model="posts.tambah.title"
                  :rules="nameRules"
                  :counter="100"
                  label="title"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="5" md="4">
                <v-text-field
                  v-model="posts.tambah.img"
                  :rules="nameRules"
                  :counter="10000"
                  label="img"
                ></v-text-field>
              </v-col>
              <v-col cols="5" md="4">
                <v-text-field
                  v-model="posts.tambah.price"
                  :rules="nameRules"
                  :counter="90"
                  label="price"
                ></v-text-field>
              </v-col>
              <v-col cols="5" md="4">
                <v-text-field
                  v-model="posts.tambah.rating"
                  :rules="nameRules"
                  :counter="90"
                  label="Rating"
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
        </v-form>
        <v-divider>this dividing</v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="dialog = false"> Close </v-btn>
          <v-btn color="primary" text @click="postData"> Add</v-btn>
        </v-card-actions>
      </v-card>
      <template v-slot:activator="{ on, attrs }">
        <div class="text-right mr-5">
          <v-btn
            class="align-right"
            color="blue lighten-4"
            dark
            v-bind="attrs"
            v-on="on"
            fab
            large
            elevation="3"
          >
            <v-icon large color="blue">mdi-plus</v-icon>
          </v-btn>
        </div>
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
        rating: "",
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
          this.dialog = false;
          this.getData();
          alert("Sukses Insert Data");
        },
        (error) => {
          console.log(error);
        }
      );
    },
    deleteData(data = null) {
      this.$axios
        .delete("http://localhost:3100/foodie/" + data.id)
        .then((response) => {
          alert("Berhasil Delete Data ke" + data.id);
          this.getData();
        });
    },
  },
};
</script>

<style>
</style>
