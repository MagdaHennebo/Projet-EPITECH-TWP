<template>
<div>
<header class="flex justify-center  h-32"> <img src="@/assets/TREBBO_hor.png" class="h-32" title="Trebbo is very beautiful"></header>
  <div class="m-9">
    <link href="https://fonts.googleapis.com/css2?family=Lato:wght@300;400&display=swap" rel="stylesheet">
    <crudList />
  </div >
  <footer
  class="invisible lg:visible absolute inset-x-0 bottom-0 flex flex-row justify-between items-end px-10 h-32 text-gray-600">
  <p class="w-80 pb-12">© Benoit Berlanger, Baptiste Cardoso, Magda Hennebo, Chrystal Tiran</p> <img src="@/assets/TREBBO.png" class="h-32 pr-80" title="Trebbo is very beautiful">
  <a class="pb-12" href="#">Contact us</a></footer>
  </div>
</template>

<script>
import crudList from '@/components/crudList.vue'
export default {
  name: 'Home',
  components: {
    crudList
  }
}
</script>

<style>
button {
  color: #ee3ec9
}
div {
  font-family: 'Lato', sans-serif;
}
a:hover {
  color: #ee3ec9;
}

/* body {
  background: url("../assets/2.jpg") fixed no-repeat center;
  background-size:cover;
} */
</style>
