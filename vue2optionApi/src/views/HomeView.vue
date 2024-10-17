<script >
import { defineComponent } from 'vue'

export default defineComponent({
  data() {
    return {

    };
  },
  mounted() {
  },
  methods: {
    buttonSubmitted() {
      this.$emit("submitted")
      console.log("clicked")
    }
  }
});
</script>

<template>
  <main>
    <button @click="buttonSubmitted">submit here</button>
  </main>
</template>
