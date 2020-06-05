<template>
  <div class="xl:flex xl:flex-col xl:justify-center xl:items-center xl:h-full">
    <section class="md:flex">
      <article 
        v-for="category in categories"
        :key="category.title"
        class="flex-1">
        <h2>{{category.title}}</h2>
        <div v-html="category.text"></div>
      </article>
    </section>

    <div class="mt-6 w-full text-center p-2 bg-gray-200 border-b-2 shadow border-wonder-green rounded xl:w-auto xl:px-10">
      <strong>Vous avez un projet? Contactez-moi par email :</strong><br/>
      <a href="mailto:hello@dev-julia.com">hello[at]dev-julia.com</a>
    </div>

    <div class="w-full mt-8">
      <h2>Quelques projets :</h2>
      <client-only>
        <vue-picture-swipe 
          class="gallery"
          :items="items"
          :options="galleryOption"></vue-picture-swipe>
      </client-only>
    </div>
  </div>
</template>

<script>
export default {
  props: ['gallery' , 'categories'],
  data() {
    return {
      items: [],
      galleryOption: {
        shareEl: false,
        bgOpacity: 0.9,
        fullscreenEl: false,
      }
    }
  },
  mounted() {
    this.gallery.forEach(item => {
      this.items.push({
        src: process.env.ROOT_URL + item.path,
        thumbnail: process.env.ROOT_URL + '/' + item.meta.thumbnail.path,
        w: item.meta.width,
        h: item.meta.height,
      })
    })
  }
}
</script>

<style lang="scss" scoped>
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

h2 {
  @apply uppercase text-center text-xl font-semibold text-dark-blue border-b-2 border-dark-blue mb-3;
}

/deep/ .my-gallery {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;

  figure {
    width: calc((100% / 3) - 10px);
    height: 100px;
    border: 1px solid #34495e;

    @screen md {
      height: 300px;
    }

    img {
      object-fit: cover;
      height: 100%;
      width: 100%;
    }
  }
}
</style>