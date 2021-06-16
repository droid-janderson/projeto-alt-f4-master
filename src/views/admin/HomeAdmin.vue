<template>
  <div class="container-inicial">
    <sidebar-titulo nomeTitulo="Notícias"></sidebar-titulo>
      <table class="container table table-bordered table-striped table-hover"  style="font-size: 20px; margin: 50px auto">
        <thead>
          <tr class="uppercase">
            <th>Titulo</th>
            <th>Autor</th>
            <th>Data</th>
          </tr>
        </thead>
        <tbody v-for="noticia in noticias" :key="noticia.id">
          <tr>
            <td>{{noticia.data().titulo}}</td>
            <td>{{noticia.data().autor}}</td>
            <td>{{noticia.data().data}}</td>
            <b-button @click="EditNoticia()">Editar</b-button>
          </tr>
        </tbody>
      </table>
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
          this.noticias.push(noticia)
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
