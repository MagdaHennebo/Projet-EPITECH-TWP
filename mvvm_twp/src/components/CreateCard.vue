<template>
    <div><form action="" method="post" @submit.prevent="createPost" ><button type="submit">+</button></form></div>
</template>

<script>
export default {
  name: 'CreateCard'
}
</script>

<style scoped>
body{

  background-color:rgb(238, 247, 238);
}
.board{
    display:flex;
    justify-content: flex-start;
    margin: 1.2rem 0.8rem;
}
.lane{
    background: var(--color-grey);
    height:30rem;
    margin:0 0.8rem;
    width: 23rem;
    border-radius:0.8rem;
    box-shadow:0 0.1rem 0.2rem 0 rgba(33, 33, 33, 0.1);
    padding:0 0.7rem;
}
.lane-title{
    padding: 0.8rem 0.5rem;
    margin-bottom: 0.6rem;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
