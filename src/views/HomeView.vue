<template>
  <div class="home">
    <CarForm class="car-form" />
  </div>
</template>

<script>
import CarForm from "../components/CarForm.vue"

export default {
  name: 'HomeView',
  components: { CarForm }
}
</script>

<style scoped>
  .car-form{
    margin-top: 34px;
  }
</style>