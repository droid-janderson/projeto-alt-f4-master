<template>
  <div>
    <!-- crrossel  -->
    <b-carousel
      id="carousel-1"
      v-model="slide"
      :interval="3500"
      controls
      indicators
      img-width="1024"
      img-height="460"
      @sliding-start="onSlideStart"
      @sliding-end="onSlideEnd"
      @reset="0.1"
      class="mt-3"
    >
      <!-- Text slides with image -->
      <b-carousel-slide 
        v-for="card in cards" 
        :key="card.id" 
        v-bind:img-src="card.data().imgDest"
      ></b-carousel-slide>

      <!-- Slides with custom text -->
    </b-carousel>

    <div class="container">
      <h2 class="text-center mb-4">Notícias</h2>

      <section class="p-0 row justify-content-md-around">
        <div v-for="card in cards" :key="card.id">
          <b-card
            v-bind:img-src="card.data().cardImg"
            v-bind:img-alt="card.data().titulo"
            img-top
            tag="article"
            style="max-width: 18rem; cursor: pointer"
            class="mb-4"
            @click="showDetails(card.id)"
          >
            <h4>{{ card.data().titulo }}</h4>
            <p>{{ card.data().autor }}</p>
          </b-card>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import moment from 'moment'

export default {
  name: 'Home',
  components: {},
  data () {
    return {
      slide: 0,
      sliding: null,
      cards: []
    }
  },

  mounted () {
    this.fetchCards()
  },

  methods: {
    onSlideStart (slide) {
      this.sliding = true
    },
    onSlideEnd (slide) {
      this.sliding = false
    },

    showDetails (id) {
      this.$router.push({ name: 'Noticia', params: { id } })
    },

    async fetchNoticias () {
      return await this.$firebase
        .firestore()
        .collection('noticias')
        .where("selecao", "array-contains", "noticia")
        .limit(3)
        .get()
    },

    async fetchCards () {
      const dataAtual = moment().format("YYYY-MM-DD")
      try {
        const cards = await this.fetchNoticias()

        if (cards.length === 0) {
          return
        }

        cards.forEach(card => {
          const dataCard = card.data().data;  
          
          if(dataCard > dataAtual) {
            return;
          }
          
          this.cards.push(card)
        })

      } catch (err) {
        console.log(err)
      }
    }
  }
}
</script>

<style scoped>

  .container {
    margin-top: 50px;
    margin-bottom: 50px;
  }
  .container h2 {
    font-size: 56px;
    font-weight: 500;
  }

</style>
