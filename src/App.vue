<template>
  <div id="app">
    <img id="mainImg" :src="fields.image.url">
    <p id="mainText">{{fields.main_text}}</p>
    <PostComponent
      v-for="post in fields.post_group"
      v-bind:id="post.post.id"
      v-bind:key="post.post.id"
    />
    <MenuBar />
  </div>
</template>

<script>
import PostComponent from './components/PostComponent.vue'
import MenuBar from './components/MenuBar.vue'

export default {
  name: 'app',
  components: {
    PostComponent,
    MenuBar
  },
  created: function() {
    this.getContent()
  },
  data() {
    return {
      fields: {
        main_text: null,
        image: {},
        post_group: []
      }
    }
  },
  methods: {
    getContent() {
      this.$prismic.client.getSingle("homepage")
        .then((document) => {
            /* eslint-disable no-console */
            console.log('homepage', document)
            this.fields = document.data
            /* eslint-disable no-console */
            return(document.data)
        })
    }
  }
}
</script>

<style>
body {
  margin: 0;
  padding: 0;
}
#mainImg {
  height: 300px;
  width: 300px;
  margin-top: 200px;
  margin-bottom: 30px;
}
#mainText {
  font-size: 24px;
  padding-bottom:100px;
  margin-bottom:100px;

}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* height: 100vh;
  width: 100vw; */
}
</style>
