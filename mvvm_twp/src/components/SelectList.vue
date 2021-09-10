<template>
    <div>
    <select @change="changecat($event)">
      <option value="">Move to</option>
      <option  v-for="(list) in lists" :key="list.id" name="lists" :value="list.id">{{list.name}}</option>
    </select>
    <!-- <button @click="changecat(list.id)">submit</button> -->
    </div>
</template>

<script>
import axios from 'axios'
export default {
  props: {
    cards: {
      type: Array,
      default: () => []
    }
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
        /* console.log(this.cards.id) */
      })
  },
  methods: {
    changecat (event) {
      console.log(this.cards.id)
      console.log(event.target.value)
      axios({
        method: 'put',
        url: 'https://twp.oxy-development.fr/wp-json/wp/v2/posts/' + this.cards.id,
        data: {
          categories: event.target.value,
          status: 'publish'
        },
        headers: {
          Authorization: 'Basic ZXBpdGVjaDpFcGl0ZWNoMTM='
        }
      })
        .then(() => {
          axios
            .get('https://twp.oxy-development.fr/wp-json/wp/v2/posts?categories=' + event.target.value + '&per_page=42')
            .then(response => {
              console.log(response)
            })
        })
    }
  }
}
</script>

<style scoped>

</style>
