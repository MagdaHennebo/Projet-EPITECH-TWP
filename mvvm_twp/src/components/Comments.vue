<template>
<div>
  <p>Activity</p>
  <div v-for="(comment, index) in comments" :key="comment.content" class="border-b-4">
    <div>
      <input v-model="comment.author_name" type="text" class="w-96 text-xl bg-gray-300 rounded-md">
    </div>
      <input  id="removeTag" v-model="comment.content.rendered" type="text" class="w-96 border bg-gray-300 rounded-md">
        <button class="hover" title = "Delete comment" v-on:click="deleteComment(comment.id)" type="button"><svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
</svg></button>
        <button class="hover" title = "Update comment" v-on:click="updateComment(comment.id, index)" type="button"><svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.232 5.232l3.536 3.536m-2.036-5.036a2.5 2.5 0 113.536 3.536L6.5 21.036H3v-3.572L16.732 3.732z" />
</svg></button>
  </div>
      <div>
        <br/>
        <input v-model="author_name" type="text" placeholder="Your name" class="w-96 bg-gray-50 rounded-md">
      </div>
      <div>
        <textarea v-model="content" placeholder="Write a comment..." class="w-96 bg-gray-50 rounded-md"></textarea>
      </div>
      <button class="hover add" title = "Add comment" v-on:click="createComment()" type="button"><svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
</svg></button>
</div>
</template>

<script>
import axios from 'axios'
/* const rmTag = document.getElementById('removeTag')
console.log(rmTag.value.slice(3, -4)) */

export default {
  name: 'Comments',
  props: {
    cards: {
      type: Array,
      default: () => []
    }
  },
  mounted () {
    axios
      .get('http://twp.oxy-development.fr/wp-json/wp/v2/comments?post=' + this.cards + '&per_page=42')
      .then(response => {
        this.comments = response.data
      })
  },
  data () {
    return {
      comments: []
    }
  },
  methods: {
    createComment () {
      axios({
        method: 'post',
        url: 'http://twp.oxy-development.fr/wp-json/wp/v2/comments',
        data: { author_name: this.author_name, content: this.content, post: this.cards, author: 1 },
        headers: {
          Authorization: 'Basic ZXBpdGVjaDpFcGl0ZWNoMTM='
        }
      })
        .then(() => {
          axios
            .get('http://twp.oxy-development.fr/wp-json/wp/v2/comments?post=' + this.cards)
            .then(response => {
              this.comments = response.data
            })
        })
    },
    deleteComment (id) {
      axios({
        method: 'delete',
        url: 'http://twp.oxy-development.fr/wp-json/wp/v2/comments/' + id,
        headers: {
          Authorization: 'Basic ZXBpdGVjaDpFcGl0ZWNoMTM='
        }
      })
        .then(() => {
          axios
            .get('http://twp.oxy-development.fr/wp-json/wp/v2/comments?post=' + this.cards + '&per_page=42')
            .then(response => {
              this.comments = response.data
            })
        })
    },
    updateComment (id, index) {
      axios({
        method: 'put',
        url: 'http://twp.oxy-development.fr/wp-json/wp/v2/comments/' + id,
        data: { author_name: this.comments[index].author_name, content: this.comments[index].content.rendered, post: this.cards },
        headers: {
          Authorization: 'Basic ZXBpdGVjaDpFcGl0ZWNoMTM='
        }
      })
        .then(() => {
          axios
            .get('http://twp.oxy-development.fr/wp-json/wp/v2/comments?post=' + this.cards + '&per_page=42')
            .then(response => {
              this.comments = response.data
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

.hover:hover {
  color: #00c4cc
}

.add {
  margin-left:80%
}
</style>
