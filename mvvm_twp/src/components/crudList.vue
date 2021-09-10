<template>
<div class="flex flex-col lg:flex-row gap-5 text-gray-600 ">
  <div v-for="(list, index) in lists" :key="list.id">
    <div class="list bg-indigo-50 w-72 rounded-lg shadow-lg pb-4">
      <div class="grid grid-cols-2 ">
        <input type="text" v-model="list.name" class="font-bold listname bg-indigo-50 p-4 rounded-lg w-44">
        <div class="flex flex-row justify-end pr-4">
          <button class="hover" title = "Delete List" v-on:click="deleteList(list.id)"><svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
</svg></button>
          <button class="hover" title = "Update List" v-on:click="updateList(list.id, index)"><svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.232 5.232l3.536 3.536m-2.036-5.036a2.5 2.5 0 113.536 3.536L6.5 21.036H3v-3.572L16.732 3.732z" />
</svg></button>
        </div>
      </div>
        <div class="pl-4">
            <BoardCards :lists="list.id"/>
        </div>
    </div>
  </div>
  <div class="flex items-start">
  <div class="flex flex-row items-center gap-1 pt-1">
      <input class="border-2 border-gray-400 rounded-lg shadow-md h-10" v-model="name">
      <button class="hover" title = "Add List" v-on:click="createList()"><svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v3m0 0v3m0-3h3m-3 0H9m12 0a9 9 0 11-18 0 9 9 0 0118 0z" />
</svg></button>
  </div>
  </div>
</div>
</template>

<script>
import axios from 'axios'
import BoardCards from '@/components/BoardCards.vue'
export default {
  name: 'Categories',
  components: {
    BoardCards
  },
  data () {
    return {
      lists: []
    }
  },
  mounted () {
    axios.get('https://twp.oxy-development.fr/wp-json/wp/v2/categories?per_page=42&orderby=id')
      .then((response) => {
        var result = []
        for (const cat of response.data) {
          if (cat.id !== 1) {
            result.push(cat)
          }
        }
        this.lists = result
      })
  },
  methods: {
    deleteList (id) {
      axios.delete('https://twp.oxy-development.fr/wp-json/wp/v2/categories/' + id + '?force=true',
        {
          headers: {
            Authorization: 'Basic ZXBpdGVjaDpFcGl0ZWNoMTM='
          }
        })
        .then((response) => {
          axios.get('https://twp.oxy-development.fr/wp-json/wp/v2/categories?per_page=42&orderby=id')
            .then((response) => {
              var result = []
              for (const cat of response.data) {
                if (cat.id !== 1) {
                  result.push(cat)
                }
              }
              this.lists = result
            })
        })
    },
    createList () {
      axios({
        method: 'post',
        url: 'https://twp.oxy-development.fr/wp-json/wp/v2/categories',
        data: { name: this.name },
        headers: {
          Authorization: 'Basic ZXBpdGVjaDpFcGl0ZWNoMTM='
        }
      })
        .then((response) => {
          axios.get('https://twp.oxy-development.fr/wp-json/wp/v2/categories?per_page=42&orderby=id')
            .then((response) => {
              var result = []
              for (const cat of response.data) {
                if (cat.id !== 1) {
                  result.push(cat)
                }
              }
              this.lists = result
            })
        })
    },
    updateList (id, index) {
      axios({
        method: 'put',
        url: 'https://twp.oxy-development.fr/wp-json/wp/v2/categories/' + id,
        data: { name: this.lists[index].name },
        headers: {
          Authorization: 'Basic ZXBpdGVjaDpFcGl0ZWNoMTM='
        }
      })
        .then((response) => {
          axios.get('https://twp.oxy-development.fr/wp-json/wp/v2/categories?per_page=42&orderby=id')
            .then((response) => {
              var result = []
              for (const cat of response.data) {
                if (cat.id !== 1) {
                  result.push(cat)
                }
              }
              this.lists = result
            })
        })
    }
  }
}

</script>

<style>
.hover {
  color: #ee3ec9
}
.list{
    background: rgb(204, 225, 233);
    max-height:40rem;
    margin:0 0.8rem;
    overflow: auto;
    border-radius:0.8rem;
    box-shadow:0 0.1rem 0.2rem 0 rgba(33, 33, 33, 0.1);
    padding:0 0.7rem;
}

.hover:hover {
  color: #00c4cc
}
</style>
