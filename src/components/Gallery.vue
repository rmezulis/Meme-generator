<template>
  <div class="container">
    <b-row class="mt-5" v-if="!images.length">
      <b-col>
        <b-spinner label="Loading images" variant="primary"></b-spinner>
      </b-col>
    </b-row>
    <div class="gallery">
      <template v-for="image in images">
        <img
          :src="image.url"
          :key="image.id"
          :alt="`Image of ${image.name} meme`"
          class="meme-thumb"
          v-b-modal.modal-1
          @click="imgToEdit = image"
        />
      </template>
    </div>
    <b-modal v-slot:modal-footer="{ }" id="modal-1" size="lg" title="Editor">
      <Editor :imgToEdit="imgToEdit" />
    </b-modal>
  </div>
</template>
<script>
import Editor from "@/components/Editor.vue";
export default {
  name: "Gallery",
  components: { Editor },
  data() {
    return {
      images: [],
      imgToEdit: {}
    };
  },
  computed: {
    cssVars() {
      return {
        '--height': this.imgToEdit.height + 'px'
      }
    }
  },
  created() {
    fetch("https://api.imgflip.com/get_memes")
      .then(response => response.json())
      .then(response => (this.images = response.data.memes))
      .catch(err => console.log(err));
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.gallery {
  line-height: 0;
  column-count: 4;
  column-gap: 0px;
}

.meme-thumb {
  width: 100% !important;
  height: auto !important;
}

.meme-thumb:hover {
  opacity: 0.5;
}

@media (max-width: 1200px) {
  .gallery {
    column-count: 3;
  }
}
@media (max-width: 1000px) {
  .gallery {
    column-count: 2;
  }
}
@media (max-width: 600px) {
  .gallery {
    column-count: 1;
  }
}
</style>
