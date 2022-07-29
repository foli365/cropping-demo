<template>
  <div>
    <div class="cropper-wrapper q-mx-auto">
      <cropper :src="imageSrc" @change="onChange" />
    </div>
    <div class="col flex justify-center content-end q-mt-md">
      <q-btn class="q-mx-sm" color="primary" rounded @click="clicked">
        Select Image
      </q-btn>
      <q-btn class="q-mx-sm" color="grey" rounded @click="fileCleared"
        >Clear</q-btn
      >
      <q-input
        type="file"
        ref="imageInput"
        accept=".jpeg,.png,.jpg"
        @change="fileChanged"
        :style="{ display: 'none' }"
      />
    </div>
    <div class="text-center q-mt-lg">
      <span>Selected File</span>
      <span v-if="selectedFile">{{ selectedFile.name }}</span>
    </div>
    <div class="q-mx-auto">
      <img :src="croppedImage" alt="" />
    </div>
  </div>
</template>

<script>
import { Cropper } from "vue-advanced-cropper";
import "vue-advanced-cropper/dist/style.css";
export default {
  name: "ImageCropper",
  components: {
    Cropper
  },
  data() {
    return {
      selectedFile: null,
      imageSrc: null,
      croppedImage: null,
      cropper: null,
      coordinates: {
        width: 0,
        height: 0,
        left: 0,
        top: 0
      }
    };
  },
  watch: {
    selectedFile: function () {
      let fileReader = new FileReader();
      if (this.selectedFile) {
        fileReader.readAsDataURL(this.selectedFile);
        fileReader.onload = (e) => {
          this.imageSrc = e.target.result;
        };
      } else {
        this.imageSrc = null;
      }
    }
  },
  methods: {
    onChange({ coordinates, canvas }) {
      this.coordinates = coordinates;
      // You able to do different manipulations at a canvas
      // but there we just get a cropped image, that can be used
      // as src for <img/> to preview result
      this.croppedImage = canvas.toDataURL();
    },
    fileChanged(e) {
      const files = e.target.files || e.dataTransfer.files;
      if (files.length) {
        this.selectedFile = files[0];
      }
    },
    fileCleared() {
      this.selectedFile = null;
      this.croppedImage = null;
    },
    clicked() {
      this.$refs.imageInput.$refs.input.click();
    }
  }
};
</script>

<style lang="scss" scoped>
img {
  height: 50%;
  width: 50%;
  max-width: 600px;
  max-height: 600px;
}
.cropper-wrapper {
  max-width: 640px;
  max-height: 480px;
  background: #282c34;
  overflow: hidden;
}
</style>
