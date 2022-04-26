<template>
  <div>
    <h1>Product</h1>
    <custom-card title="faisal affan" img="https://www.themoviedb.org/t/p/w600_and_h900_bestv2/lJA2RCMfsWoskqlQhXPSLFQGXEJ.jpg"></custom-card>
    <custom-card title="fahmi" img="https://www.themoviedb.org/t/p/w600_and_h900_bestv2/x6FsYvt33846IQnDSFxla9j0RX8.jpg"></custom-card>
  </div>
</template>

<script>
import customCard from "./_card.vue";
export default {
  components: {
    customCard: customCard,
  },
};
</script>

<style></style>
