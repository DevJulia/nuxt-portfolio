<template>
  <div class="xl:flex xl:flex-col xl:justify-center xl:items-center text-justify xl:h-full">
    <div class="mb-5" v-html="data.carriere"></div>
    <a class="block text-center linkedin" :href="data.linkedin" target="_blank" rel="external"><fa class="h-4" :icon="['fab', 'linkedin']" /> <span class="underline">LinkedIn</span></a>
    <a class="block text-center underline" :href="`${root_url}/${data.cv}`">Télécharger mon CV</a>
  </div> 
</template>

<script>
export default {
  props: ['data'],
  computed: {
    root_url() {
      return process.env.rootUrl;
    }
  },
}
</script>

<style lang="scss" scoped>
svg {
  display: inline;
}

/deep/ p {
  @apply mb-6;
}

a {
  &.linkedin {
    color: #0077B5;
  }
}
</style>