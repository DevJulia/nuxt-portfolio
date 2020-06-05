<template>
  <div class="masonry xl:h-full">
    <div 
      class="item"
      v-for="image in gallery"
      :key="image.meta.asset">
      <img 
        :src="`${root_url}/${image.path}`" 
        :alt="image.meta.title"
        :title="image.meta.title">
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  props: ['gallery'],
  computed: {
    root_url() {
      return process.env.rootUrl;
    }
  }
}
</script>

<style lang="scss" scoped>
.masonry {
  margin-top: 1em;
  max-width: 1200px;
  margin-left: auto;
  margin-right: auto;
  column-gap: 0.5em;
  column-count: 2;

  .item {
    display: inline-block;
    width: 100%;
    overflow: hidden;

    img {
      max-width: 100%;
      height: auto;
      display: block;
      border-radius: 3px;
    }
  }

  @screen md {
    column-count: 4;
  }
}
</style>