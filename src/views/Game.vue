<template>
  <div class="container" v-html="noticia[0]"></div>
</template>

<script>
export default {
  data () {
    return {
      noticia: []
    }
  },

  mounted() {
    const db = this.$firebase.firestore();
    db
      .collection('noticias')
      .get()
      .then(snap => {
        const Url = window.location.href

        const idUrl = Url.split('game/')[1]

        snap.forEach(noticia => {
          if(noticia.id === idUrl) {

            this.noticia.push(noticia.data().editorData)
            console.log(noticia.data().editorData)
          }
        })
      });

  }
}
</script>

<style lang="scss" scoped>
.container {
  margin: 50px auto;
}

p span {
  font-size: small;
}

p {
  font-size: 20px;
  text-align: start;
  line-height: 1.5;
  padding-bottom: 15px;
}

.logo-rodape,
h3 {
  font-size: 28px;
}

b {
  font-size: 25px;
  padding: 20px;
}

img {
  width: 30px;
}

.content-right a {
  padding: 10px;
}
</style>
