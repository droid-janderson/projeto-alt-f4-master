<template>
  <div class="container-inicial">
    <sidebar-titulo nomeTitulo="Notícias"></sidebar-titulo>
    <b-table striped hover :items="noticias">
    </b-table>
  </div>
</template>

<script>
import SidebarTitulo from '../../components/SidebarTitulo.vue'

export default {
  components: {
    SidebarTitulo
  },
  data () {
    return {
      noticias: []
    }
  },

  mounted() {
    this.fetchCards()
  },

  methods: {
    async fetchNoticias () {
      return await this.$firebase
      .firestore()
      .collection('noticias')
      .get()
    },

    async fetchCards () { 
      try { 
        const noticias = await this.fetchNoticias() 

        if (noticias.length === 0) {
          return 
        } 
        
        noticias.forEach(noticia => {
          this.noticias.push(noticia.data())
        }) 
      } catch (err) {
        console.log(err)
      }
    }
  }
}
</script>

<style scoped>
.container-inicial {
  width: 100vw;
  height: 100vh;
}
</style>
