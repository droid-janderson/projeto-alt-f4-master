<template>
  <div class="container">
    <section class="p-0 row justify-content-md-around">
      <div v-if="isLoading">
        <b-button variant="primary" disabled>
          <b-spinner small type="grow"></b-spinner>
          Loading...
        </b-button>
      </div>

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
        </b-card>
      </div>
    </section>
  </div>
</template>

<script>
import moment from 'moment'

export default {
  data () {
    return {
      cards: [],
      isLoading: true
    }
  },

  mounted () {
    this.fetchCards()
  },

  methods: {
    showDetails (id) {
      this.$router.push({ name: 'Game', params: { id } })
    },

    setLoading (state) {
      if (typeof state !== 'boolean') {
        return
      }

      this.isLoading = state
    },

    async fetchNoticias () {
      return await this.$firebase
        .firestore()
        .collection('noticias')
        .where('selecao', 'array-contains', 'game') 
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

        this.setLoading(false)
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

</style>
