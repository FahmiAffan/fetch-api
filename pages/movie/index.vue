<template>
  <div>
    <h1>Movies</h1>
    <v-text-field
      v-model="filter.movie"
      :counter="10"
      label="Search Movie"
      required
    ></v-text-field>
    <!-- TAMBAH -->
    <v-dialog v-model="modal.dialogTambah" width="500" persistent>
      <template v-slot:activator="{ on, attrs }">
        <v-btn color="red lighten-2" dark v-bind="attrs" v-on="on">
          Tambah
        </v-btn>
      </template>

      <v-card>
        <v-card-title class="text-h5 grey lighten-2">
          Insert Data
        </v-card-title>

        <v-card-text>
          <v-form v-model="modal.tambahValid">
            <v-container>
              <v-row>
                <v-col cols="12">
                  <v-text-field
                    v-model="modal.tambah.title"
                    :counter="10"
                    label="TItle"
                    required
                  ></v-text-field>
                </v-col>

                <v-col cols="12">
                  <v-text-field
                    v-model="modal.tambah.author"
                    :counter="10"
                    label="Author"
                    required
                  ></v-text-field>
                </v-col>
                <v-col cols="12">
                  <v-text-field
                    v-model="modal.tambah.img"
                    :counter="10"
                    label="Image"
                    required
                  ></v-text-field>
                </v-col>

              </v-row>
            </v-container>
          </v-form>
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-btn color="primary" text @click="modal.dialogTambah = false">
            CLOSE
          </v-btn>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="postData"> ADD </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- UPDATE -->
    <v-dialog v-model="modal.dialogUpdate" width="500" persistent>
      <v-card>
        <v-card-title class="text-h5 grey lighten-2">
          Update Data {{ modal.edit.id }}
        </v-card-title>

        <v-card-text>
          <v-form v-model="modal.editValid">
            <v-container>
              <v-row>
                <v-col cols="12">
                  <v-text-field
                    v-model="modal.edit.title"
                    :counter="10"
                    label="TItle"
                    required
                  ></v-text-field>
                </v-col>

                <v-col cols="12">
                  <v-text-field
                    v-model="modal.edit.author"
                    :counter="10"
                    label="Author"
                    required
                  ></v-text-field>
                </v-col>
                <v-col cols="12">
                  <v-text-field
                    v-model="modal.edit.img"
                    :counter="10"
                    label="Image"
                    required
                  ></v-text-field>
                </v-col>
              </v-row>
            </v-container>
          </v-form>
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-btn color="primary" text @click="modal.dialogUpdate = false">
            CLOSE
          </v-btn>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="updateData(false, null)">
            UPDATE
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-simple-table>
      <template v-slot:default>
        <thead>
          <tr>
            <th class="text-left">Title</th>
            <th class="text-left">Author</th>
            <th class="text-left" colspan="2">Author</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, i) in movieComputed" :key="i">
            <td>{{ item.title }}</td>
            <td>{{ item.author }}</td>
            <td>
              <v-btn color="red lighten-2" dark @click="updateData(true, item)">
                Edit
              </v-btn>
              <v-btn color="red lighten-2" dark @click="deleteData(item)">
                Delete
              </v-btn>
            </td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>
    <v-row>
      <v-col cols="4" v-for="(item, i) in movieComputed" :key="i">
        <card-custom
          :title="item.title"
          :img="item.img"
          :author="item.author"
          :dataLengkap="item"
          des="Ingpo"
          @klikUpdate="updateData"
          @klikDelete="deleteData"
        ></card-custom>
      </v-col>
    </v-row>
    <!-- <card-custom title="Web Development" des="Halo masse"></card-custom> -->
  </div>
</template>

<script>
import axios from "axios";
// import vue from 'vue';
import CardCustom from "./_card.vue";

export default {
  name: "ThisMovies",
  data: () => ({
    filter: {
      movie: "",
    },
    movies: [],
    modal: {
      dialogTambah: false,
      dialogUpdate: false,
      tambahValid: false,
      tambah: {
        title: "",
        author: "",
        img : ""
      },
      edit: {
        id: null,
        title: "",
        author: "",
        img : ""
      },
    },
    rules: {
      nameRules: [
        (v) => !!v || "Name is required",
        (v) => v.length <= 10 || "Name must be less than 10 characters",
      ],
    },
  }),
  components: {
    CardCustom,
  },
  created() {
    // console.log('faisal');
    this.getData();
  },
  computed: {
    movieComputed() {
      if (this.filter.movie == "" || this.filter.movie == null) {
        return this.movies;
      } else {
        let result = this.movies;
        return result.filter((e) => {
          return (
            e.title.toLowerCase().includes(this.filter.movie.toLowerCase()) ||
            e.author.toLowerCase().includes(this.filter.movie.toLowerCase())
          );
        });
      }
    },
  },
  methods: {
    getData() {
      axios
        .get("http://localhost:3100/movies")
        .then((response) => {
          this.movies = response.data; 
        })
        .catch((err) => {
          console.log(err);
        });
    },
    async postData() {
      // alert("halow");
      // console.log(this.modal.tambah);
      let payload = this.modal.tambah;
      axios
        .post("http://localhost:3100/movies", payload)
        .then((response) => {
          console.log(response);
          this.modal.dialogTambah = false;
          this.getData();
          alert("Success insert data");
        })
        .catch((err) => console.log(err));
    },
    deleteData(data = null) {
      // alert(data.id);
      // jika tidak muncul modal maka jalankan HTTP REQ
      this.$axios
        .delete("http://localhost:3100/movies/" + data.id)
        .then((res) => {
          alert("Berhasil Delete Data " + data.id);
          this.getData();
        });
    },
    updateData(modal = false, data = null) {
      // jika tidak muncul modal maka jalankan HTTP REQ
      if (modal == false) {
        let payload = this.modal.edit;
        // alert(JSON.stringify(payload));
        this.$axios
          .put("http://localhost:3100/movies/" + payload.id, payload)
          .then((res) => {
            this.modal.dialogUpdate = false;
            alert("Berhasil Update Data " + payload.id);
            this.getData();
            this.modal.edit = {
              id: null,
              title: "",
              author: "",
              img:""
            };
          });
      } else {
        // console.log(data);
        this.modal.edit = {
          id: data.id,
          title: data.title,
          author: data.author,
        };
        this.modal.dialogUpdate = true;
      }
    },
  },
};
</script>

<style>
</style>
