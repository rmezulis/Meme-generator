<template>
  <div class="editor d-flex">
    <div class="image col" id="download">
      <img :src="imgToEdit.url" :alt="imgToEdit.name" class="image-to-edit" />
      <div v-for="input in inputs" :key="input.id">
        <Moveable :for="input.id" class="moveable" v-bind="moveable" @drag="handleDrag">
          <span :style="{fontSize: `${input.fontSize}px`, color: textColor}">{{input.message}}</span>
        </Moveable>
      </div>
    </div>
    <div class="edit-options container col">
      <b-button class="row" @click="addInput()">Add input</b-button>
      <div class="text-line" v-for="input in inputs" :key="input.id">
        <label :for="input.id" class="row">Enter your text:</label>
        <input :id="input.id" class="row" v-model="input.message" placeholder="Type here" />
        <p class="row">Font size:</p>
        <vue-slider class="row" :min="10" :max="50" v-model="input.fontSize"></vue-slider>
      </div>
      
      <div class="color-picker row">
        <p>Text color:</p>
        <verte picker="square" model="rgb" v-model="textColor"></verte>
      </div>
      <div class="row">
        <input v-model="file" placeholder="Enter file name" />
        <b-button @click="download()">Download</b-button>
      </div>
    </div>
  </div>
</template>

<script>
import { saveAsJpeg } from "save-html-as-image";
import Moveable from "vue-moveable";

export default {
  name: "Gallery",
  components: {
    Moveable
  },
  props: ["imgToEdit"],
  data: function() {
    return {
      inputAmount: 1,
      inputs: [
        {
          id: 0,
          message: "Insert your text",
          fontSize: 15
        }
      ],
      imageHeight: 300,
      textColor: "",
      moveable: {
        draggable: true
      },
      file: this.imgToEdit.name
    };
  },
  watch: {
    imgToEdit: function() {
      if (this.imgToEdit.height > 400) {
        this.imageHeight = 400;
      } else {
        this.imageHeight = this.imgToEdit.height;
      }
    }
  },
  methods: {
    download() {
      let node = document.getElementById("download");
      saveAsJpeg(node, { filename: this.file, printDate: false });
    },
    handleDrag({ target, transform }) {
      target.style.transform = transform;
    },
    addInput() {
      this.inputs.push({
        id: this.inputAmount++,
        message: "Insert your text",
        fontSize: 15
      });
    }
  }
};
</script>

<style scoped>
.image-to-edit {
  max-width: 100%;
  max-height: 100%;
}

.color-picker {
  align-content: left;
}

@media(max-width: 1000px) {
  .editor {
    flex-direction: column;
  }
}
</style>
