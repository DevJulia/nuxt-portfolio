<template>
  <div class="lg:flex">
    <sidebar class="lg:w-1/5" :data="hero" />
    <div class="content lg:w-4/5 p-6 lg:px-10 flex flex-col">
      <ul class="flex flex-wrap mb-4 shadow">
        <li 
          class="w-1/2 sm:w-1/4"
          v-for="(item,key) in menu" :key="item">
          <button
            @click="currentComponent = key"
            :class="{ active: currentComponent === key }"
            class="w-full py-1 px-3 border border-dark-blue font-semibold"
          >
            {{item}}
          </button>
        </li>
      </ul>
      <transition name="component-fade" mode="out-in">
        <component 
          :is="currentComponent"
          v-bind="currentProperties"></component>
      </transition>
      <ul class="flex flex-wrap mt-6 border border-dark-blue shadow">
        <li 
          class="w-1/2 sm:w-1/4"
          v-for="(item,key) in menu" :key="item">
          <button
            @click="currentComponent = key"
            :class="{ active: currentComponent === key }"
            class="w-full py-1 px-3 border border-dark-blue font-semibold"
          >
            {{item}}
          </button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import Sidebar from '~/components/Sidebar.vue'
import Techno from '~/components/Techno.vue'
import Projets from '~/components/Projets.vue'
import Carriere from '~/components/Carriere.vue'
import Loisirs from '~/components/Loisirs.vue'

export default {
  async asyncData ({ $axios }) {
    const hero = await $axios.$get(`${process.env.BASE_URL}/api/singletons/get/hero?token=${process.env.API_TOKEN}`);
    const services = await $axios.$get(`${process.env.BASE_URL}/api/collections/get/services?token=${process.env.API_TOKEN}`);
    const resources = await $axios.$get(`${process.env.BASE_URL}/api/singletons/get/resources?token=${process.env.API_TOKEN}`);
    const projectTypes = await $axios.$get(`${process.env.BASE_URL}/api/collections/get/projects?token=${process.env.API_TOKEN}`);

    return { hero, services, resources, projectTypes }
  },
  data() {
    return {
      currentComponent: "techno",
      menu: {
        techno: "Techno",
        projets: "Projets",
        carriere: "Carrière",
        loisirs: "Loisirs"
      },
    }
  },
  computed: {
    currentProperties() {
      switch(this.currentComponent) {
        case 'techno':
          return { 
            services: this.services.entries,
            resources: this.resources 
          }
        case 'carriere' :
          return { data: this.hero }
        case 'projets' :
          return { 
            categories : this.projectTypes.entries,
            gallery : this.resources.projects_gallery 
          }
        case 'loisirs' :
          return { gallery : this.resources.hobbies_gallery }
        default:
          return;
      }
    }
  },
  components: {
    Sidebar,
    Techno,
    Projets,
    Carriere,
    Loisirs,
  },
  created() {
    console.log('%c (ﾉ◕ヮ◕)ﾉ*:･ﾟ✧ Bonjour confrère dev! ', 'background: #000; color: #fff');
    console.log("%c Le code du site est sur Github si besoin.", 'background: #000; color: #fff');
    console.log("%c Reste curieux!", 'background: #000; color: #fff');
  }
}
</script>

<style lang="scss">
/* Sample `apply` at-rules with Tailwind CSS
.container {
  @apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/
section.content {
  min-height: 100vh; /* Fallback for browsers that do not support Custom Properties */
  min-height: calc(var(--vh, 1vh) * 100);
  background-color: #FCFBFC;
}

.component-fade-enter-active, .component-fade-leave-active {
  transition: opacity .3s ease;
}
.component-fade-enter, .component-fade-leave-to
/* .component-fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>

<style lang="scss" scoped>
button {
  transition: all .3s ease-in;
  @apply bg-gray-300;

  &:hover {
    @apply bg-gray-400;
  }

  &.active {
    @apply bg-dark-blue text-wonder-green;
  }

  &:focus {
    outline: 0;
  }
}
</style>