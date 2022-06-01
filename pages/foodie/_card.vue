<template>
  <v-card :loading="loading" class="mx-auto my-12" max-width="374">
    <template slot="progress">
      <v-progress-linear
        color="deep-purple"
        height="10"
        indeterminate
      ></v-progress-linear>
    </template>

    <v-img height="250" :src="img"></v-img>

    <v-card-title>{{ title }}</v-card-title>

    <v-card-text>
      <v-row align="center" class="mx-0">
        <v-rating
          :value="rating"
          background-color="grey lighten-2"
          length= "5"
          color="amber"
          dense
          half-increments
          readonly
          size="16"
        ></v-rating>

        <div class="grey--text ms-4">{{rating}}</div>
      </v-row>

      <div class="my-4 text-subtitle-1">Rp.{{ price }}</div>
    </v-card-text>

    <v-divider class="mx-4"></v-divider>

    <v-card-actions>
      <v-btn text color="red darken-1" @click="$emit('klikDelete', emitting)"> Delete </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  data: () => ({
    loading: false,
    selection: 1,
  }),

  methods: {
    reserve() {
      this.loading = true;

      setTimeout(() => (this.loading = false), 2000);
    },
  },
  props: {
    title: "",
    img: "",
    price:"",
    rating:"",
    emitting:{}
  },
};
</script>

<style>
</style>
