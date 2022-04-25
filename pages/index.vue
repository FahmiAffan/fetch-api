<template>
  <div>
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
          <v-btn color="primary" text @click="insertData"> ADD </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
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
          <tr v-for="(item, i) in posts" :key="i">
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
  </div>
</template>

<script>
export default {
  name: "IndexPage",
  data: () => ({
    posts: [],
    modal: {
      dialogTambah: false,
      dialogUpdate: false,
      tambahValid: false,
      tambah: {
        title: "",
        author: "",
      },
      edit: {
        id: null,
        title: "",
        author: "",
      },
    },
    rules: {
      nameRules: [
        (v) => !!v || "Name is required",
        (v) => v.length <= 10 || "Name must be less than 10 characters",
      ],
    },
  }),
  methods: {
    getData() {
      this.$axios.get("http://localhost:3100/posts").then((res) => {
        this.posts = res.data;
      });
    },
    insertData() {
      // let payload = { title: "json-server", author: "typicode" };
      let payload = this.modal.tambah;
      this.$axios.post("http://localhost:3100/posts", payload).then((res) => {
        this.modal.dialogTambah = false;
        alert("Berhasil Tambah Data");
        this.getData();
      });
    },
    updateData(modal = false, data = null) {
      // jika tidak muncul modal maka jalankan HTTP REQ
      if (modal == false) {
        let payload = this.modal.edit;
        this.$axios
          .put("http://localhost:3100/posts/" + payload.id, payload)
          .then((res) => {
            this.modal.dialogUpdate = false;
            alert("Berhasil Update Data " + payload.id);
            this.getData();
            this.modal.edit = {
              id: null,
              title: "",
              author: "",
            };
          });
      } else {
        console.log(data);
        this.modal.edit = {
          id: data.id,
          title: data.title,
          author: data.author,
        };
        this.modal.dialogUpdate = true;
      }
    },
    deleteData(data = null) {
      // jika tidak muncul modal maka jalankan HTTP REQ
      this.$axios
        .delete("http://localhost:3100/posts/" + data.id)
        .then((res) => {
          alert("Berhasil Delete Data " + data.id);
          this.getData();
        });
    },
  },
  created() {
    this.getData();
  },
};
</script>
