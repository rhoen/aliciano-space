<template>
  <div :style="cssVars" class="PostWrapper">
    <img class="post-image" :src="fields.image.url">
    <h1>{{fields.title}}</h1>
    <div>
      <prismic-rich-text :field="fields.content"/>
    </div>
  </div>
</template>

<script>
export default {
  name: 'post',
  props:['id'],
  data() {
    return {
      fields: {
        title: null,
        image: {},
        content: null,
        background_color: 'white'
      }
    }
  },
  created: function() {
    this.getContent()
  },
  methods: {
    getContent() {
      this.$prismic.client.query(this.$prismic.Predicates.at('document.id', this.id))
        .then((document) => {
            this.fields = document.results[0].data
            /* eslint-disable no-console */
            console.log('the color', this.fields.background_color)
            /* eslint-disable no-console */
        })
    }
  },
  computed: {
      cssVars() {
        return {
          'background-color': this.fields.background_color
        }
      }
    }
}
</script>

<style>
.post-image {
  width: 500px;
}
.PostWrapper {
  margin: 30px 0;
  padding-top: 30px;
  padding-bottom: 30px;
  border-top: 1px solid #5f5f61;
}
</style>
