<template>
  <streamer-text text="LIVE ON!"></streamer-text>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import StreamerText from '@/components/streamer-text.vue'

export default defineComponent({
  name: "App",
  components: {
    StreamerText
  }
});
</script>

<style lang="stylus">
</style>
