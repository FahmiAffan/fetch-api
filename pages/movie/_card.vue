<template>
  <v-card :loading="loading" class="mx-auto my-12" max-width="374">
    <template slot="progress">
      <v-progress-linear
        color="deep-purple"
        height="10"
        indeterminate
      ></v-progress-linear>
    </template>

    <div class="text-right">
      <v-menu offset-y>
        <template v-slot:activator="{ on, attrs }">
          <v-btn color="primary" dark v-bind="attrs" v-on="on" text>
            <v-icon large color="blue-grey darken-2">
              mdi-dots-horizontal
            </v-icon>
          </v-btn>
        </template>
        <v-list>
          <v-btn text @click="$emit('klikUpdate', 'true', dataLengkap)"> Edit </v-btn>
          <br>
          <v-btn text @click="$emit('klikDelete', dataLengkap)"> Delete </v-btn>
        </v-list>
      </v-menu>
    </div>
    <!-- Three-Dots -->

    <v-img height="250" :src="img"></v-img>

    <!-- Card Title -->

    <v-card-title>
      {{ title }}
    </v-card-title>

    <v-divider class="mx-5"></v-divider>
    <v-card-text>
      <div class="my-8 text-subtitle-6">by {{ author }}</div>
    </v-card-text>
  <div>
    {{des}}
  </div>
    <v-card-actions>
      <v-btn color="deep-purple lighten-2" text @click="reserve">
        Reserve
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  props: {
    title: "",
    img: "",
    des: "",
    author: "",
    dataLengkap: {}
  },
};
</script>

<style></style>
