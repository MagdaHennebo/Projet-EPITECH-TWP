<template>
  <div class="text-gray-600">
    <div class="pl-2">
          <input type="text" v-model="title" placeholder="Add a card" class="bg-gray-50 rounded-md shadow-md h-10 w-48">
          <button class="hover pr-2 pl-1 pt-1" title = "Add card" @click="createCard()" type="submit"><svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v3m0 0v3m0-3h3m-3 0H9m12 0a9 9 0 11-18 0 9 9 0 0118 0z" />
          </svg></button>
    </div>
    <div v-for="(card) in cards" :key="card.id" class="flex ">
      <!-- <h1 id="titlePost" class="">{{card.title.rendered}}</h1> -->
      <div class="m-2 border-b-2 bg-gray-50 hoverCard rounded-md">
          <div>
            <button @click="getCard(card)" class="w-48">{{card.title.rendered}}</button>
            <button title="Delete card" class="hover py-4 pr-2" @click="deleteCard(card.id)"><svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
            </svg></button>
          </div>
        <div v-if="detailCard"></div>
          <div v-else>
            <div id="detailCard" class="fixed overflow-auto top-1/4 left-1/4 bg-gray-300 h-3/5 w-1/4 p-10 pt-8 shadow-xl rounded-xl comments">
              <button title="Close card" class="hover closeCard" @click="detailCard = !detailCard"><svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z" />
              </svg>
              </button>
                <div class="mb-3 border-b-2">
                    <input @click="showedit = !showedit" type="text" name="title" v-model="popupCard.title.rendered" class="text-2xl w-96 bg-gray-200 rounded-md">
                   <div class="mt-3">
                      <p>Description</p>
                      <textarea v-if="showedit" v-model="popupCard.content.rendered" placeholder="type content here" class="w-96 bg-gray-200 rounded-md"></textarea>
                      <div @click="showedit = !showedit" v-else class=" w-96 bg-gray-200 rounded-md">{{testcontent}}</div>
                   </div>
                    <button title= "Update card" class="hover add" @click="updatePopupCard(popupCard.id); showedit = !showedit"><svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
                    </svg></button>
                </div>
                <Comments :cards="popupCard.id"/>
                <SelectList :cards="popupCard"/>
            </div>
            </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Comments from '@/components/Comments.vue'
import SelectList from '@/components/SelectList.vue'

export default {
  name: 'Cards',
  props: {
    lists: {
      type: Array,
      default: () => []
    }
  },
  components: {
    Comments, SelectList
  },
  data () {
    return {
      cards: [],
      detailCard: true,
      popupCard: [],
      showedit: false,
      testcontent: ''
    }
  },
  mounted () {
    axios
      .get('https://twp.oxy-development.fr/wp-json/wp/v2/posts?categories=' + this.lists + '&per_page=42')
      .then(response => {
        this.cards = response.data
      })
  },
  methods: {
    createCard () {
      axios({
        method: 'post',
        url: 'https://twp.oxy-development.fr/wp-json/wp/v2/posts',
        data: { title: this.title, status: 'publish', categories: [this.lists] },
        headers: {
          Authorization: 'Basic ZXBpdGVjaDpFcGl0ZWNoMTM='
        }
      })
        .then(() => {
          axios
            .get('https://twp.oxy-development.fr/wp-json/wp/v2/posts?categories=' + this.lists + '&per_page=42')
            .then(response => {
              this.cards = response.data
            })
        })
    },
    deleteCard (id) {
      axios({
        method: 'delete',
        url: 'https://twp.oxy-development.fr/wp-json/wp/v2/posts/' + id,
        headers: {
          Authorization: 'Basic ZXBpdGVjaDpFcGl0ZWNoMTM='
        }
      })
        .then(() => {
          axios
            .get('https://twp.oxy-development.fr/wp-json/wp/v2/posts?categories=' + this.lists + '&per_page=42')
            .then(response => {
              this.cards = response.data
            })
        })
    },
    updateCard (id, index) {
      axios({
        method: 'put',
        url: 'https://twp.oxy-development.fr/wp-json/wp/v2/posts/' + id,
        data: {
          title: this.cards[index].title.rendered,
          content: this.cards[index].content.rendered,
          status: 'publish'
        },
        headers: {
          Authorization: 'Basic ZXBpdGVjaDpFcGl0ZWNoMTM='
        }
      })
        .then(() => {
          axios
            .get('https://twp.oxy-development.fr/wp-json/wp/v2/posts?categories=' + this.lists + '&per_page=42')
            .then(response => {
              this.cards = response.data
            })
        })
    },
    updatePopupCard (id) {
      axios({
        method: 'put',
        url: 'https://twp.oxy-development.fr/wp-json/wp/v2/posts/' + id,
        data: {
          title: this.popupCard.title.rendered,
          content: this.popupCard.content.rendered,
          status: 'publish'
        },
        headers: {
          Authorization: 'Basic ZXBpdGVjaDpFcGl0ZWNoMTM='
        }
      })
        .then(() => {
          axios
            .get('https://twp.oxy-development.fr/wp-json/wp/v2/posts/' + id)
            .then(response => {
              this.testcontent = response.data.content.rendered.slice(3, -5)
            })
        })
    },
    getCard (card) {
      this.detailCard = !this.detailCard
      this.popupCard = card
      this.testcontent = card.content.rendered.slice(3, -5)
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

body {
  font-family: 'Lato', sans-serif;
}

.hoverCard:hover {
  box-shadow: 8px 5px 5px silver;
  background-color: #eeeeee
}

.comments {
  margin-left:15em
}

.closeCard {
  margin-left:95%
}

.add {
  margin-left:80%
}
</style>
