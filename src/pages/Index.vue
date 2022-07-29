<template>
  <div class="mx-auto">
    <h1 class="text-xl text-center">Image cropping</h1>
    <ImageCropper />
  </div>
</template>

<script>
import ImageCropper from "../components/ImageCropper.vue";
export default {
  name: "IndexPage",
  components: {
    ImageCropper
  }
};
</script>
