<template>
<div id="app">
<Header />
<div class="board">
<div class="lane">
    <h2 class="lane-title">backlog</h2>
    <Card>Card 1</Card>
</div>
<div class="lane">
    <h2 class="lane-title">backlog</h2>
    <Card>Card2</Card>
</div>
<div class="lane">
    <h2 class="lane-title">backlog</h2>
</div>
<div class="lane">
    <h2 class="lane-title">backlog</h2>
</div>
</div>
<router-view class=""/>
</div>  

</template>
<script>
import Header from '@/components/Header.vue';
import Card from '@/components/Card.vue';
import initialCards from '@/components/initialCards.vue'

export default {
    name:'App',
    components:{
            Header,Card,
            
        }  ,
        data: () => ({ 
          cards:{
            backlog:[],
            dev:[],
          }
        })
}
</script>
<style>
body{

  background-color:rgb(20, 230, 20);
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
