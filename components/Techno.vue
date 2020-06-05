<template>
  <div class="xl:flex xl:h-full xl:flex-col xl:justify-center xl:items-center">
    <div class="xl:flex xl:mb-10">
      <article 
        v-for="(service,index) in services"
        :class="serviceClass(index)"
        :key="service._id">
        <h2>{{service.title}}</h2>
        <img v-if="service.logo" :src="`${root_url}/${service.logo.path}`" :alt="service.title" />
        <div v-html="service.text"></div>
      </article>
    </div>
   
    <div class="mt-6 w-full text-center p-2 bg-gray-200 border-b-2 border-wonder-green shadow rounded xl:w-auto xl:px-10">
      Retrouvez-moi sur :
      <ul>
        <li><a :href="resources.blog" target="_blank"><fa class="h-4" :icon="['fab', 'wordpress']" /> <span class="underline">Blog</span></a></li>
        <li><a :href="resources.stack_overflow" target="_blank" rel="external"><fa class="h-4" :icon="['fab', 'stack-overflow']" /> <span class="underline">Stack Overflow</span></a></li>
        <li><a :href="resources.github" target="_blank" rel="external"><fa class="h-4" :icon="['fab', 'github']" /> <span class="underline">Github</span></a></li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  props: ['services', 'resources'],
  computed: {
    root_url() {
      return process.env.rootUrl;
    }
  },
  methods: {
    serviceClass(index) {
      switch(index) {
        case 0:
          return 'bg-orange-100 rounded shadow xl:order-1';
        case 1:
          return 'text-gray-700 xl:order-0';
        case 2:
          return 'text-gray-700 xl:order-3';
        default:
          return;
      }
    } 
  },
}
</script>

<style lang="scss" scoped>
svg {
  display: inline;
}

img {
  @apply h-20 mx-auto mb-3;
}

h2 {
  @apply uppercase text-center text-xl font-semibold text-dark-blue border-b-2 border-wonder-green mb-3;
}

article {
  @apply text-justify leading-snug mb-5 p-4;

  @screen xl {
    @apply w-1/3 mb-0;
  }

  /deep/ ul {
    @apply mt-2 text-left leading-normal;
  }

  /deep/ li:before {
    content: '';
    display: inline-block;
    height: 5px;
    width: 5px;
    border-radius: 3px;
    line-height: 1rem;
    vertical-align: middle;
    
    @apply bg-dark-blue mr-1;
  }
}
</style>