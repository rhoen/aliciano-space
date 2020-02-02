<template>
  <div class="CollectionWrapper">

  </div>
</template>

<script>

const graphQuery = `{
  collection {
    title
    description
    image_links {
      x_coordinate
      y_coordinate
      image {
        ...imageFields
      }
    }
  }
}`;
export default {
  name: 'Post',
  components: {

  },
  props: {
    title: String,
    content: String,
    imgSrc: String,
  },
  data() {
    return {
      documentId: '',
      fields: {
        title: null,
        description: null,
        images: [],
      }
    }
  },
  created: function() {
    // this.getContent()
  },
  methods: {
    getContent() {
      this.$prismic.client.getByUID('collection', 'frontpage', { 'graphQuery': graphQuery })
        .then((document) => {
          if (document && document.data && Array.isArray(document.data.image_links)) {
            const images = document.data.image_links.map(imageLink => ({
              xCoordinate: imageLink.x_coordinate,
              yCoordinate: imageLink.y_coordinate,
              src: imageLink.image.data.image.url,
              alt: imageLink.image.data.image.alt
            }));
            this.fields.images = images;
          }
        });
    }
  }
}
</script>
<style>
  .CollectionWrapper {
    overflow: scroll;
    position: relative;
    width: 100vw;
    flex-grow: 1;
  }
</style>
