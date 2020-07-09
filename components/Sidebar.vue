<template>
  <section class="sidebar lg:shadow-2xl text-center text-white bg-dark-blue">
    <div class="container h-full mx-auto py-4 px-4 flex flex-col justify-around">
      <div class="md:flex md:items-center lg:block">
        <div class="md:w-1/4 lg:w-full mb-4">
          <div class="relative w-24 md:w-32 mx-auto">
            <img 
              class="h-24 w-24 md:h-32 md:w-32 border-4 border-white shadow-xl mx-auto rounded-full object-cover mb-3"
              :src="'https://dev-julia.com/'+data.picture.path" 
              alt="">
            <a href="https://dev-julia-chat.web.app/" target="_blank" class="chat-bubble" title="Accéder au chat">
              <div class="loading">
                <div class="dot one"></div>
                <div class="dot two"></div>
                <div class="dot three"></div>
              </div>
              <div class="tail"></div>
            </a>
          </div>
  
          <div>
            <h1 class="text-2xl text-wonder-green uppercase font-semi-bold tracking-tight leading-none">{{data.title}}</h1>
            <p class="font-light leading-tight">{{data.subtitle}}</p>
            <p class="text-xs flex justify-center items-center mt-1">
              <svg class="h-3 mr-1 fill-current" viewBox="0 0 24 24"><path d="M12,11.5A2.5,2.5 0 0,1 9.5,9A2.5,2.5 0 0,1 12,6.5A2.5,2.5 0 0,1 14.5,9A2.5,2.5 0 0,1 12,11.5M12,2A7,7 0 0,0 5,9C5,14.25 12,22 12,22C12,22 19,14.25 19,9A7,7 0 0,0 12,2Z" /></svg>
              <span>{{data.location}}</span>
            </p>
          </div>
        </div>

        <div v-html="data.text" class="md:w-3/4 lg:w-full text md:mt-6"></div>
      </div>

      <div v-html="data.status" class="news border-t-2 border-b-2 border-gray-400 text-wonder-green leading-tight"></div>

      <div class="text-sm md:text-base mt-1">
        <ul>
          <li class="inline-block md:block">@ : <a class="hover:underline" :href="'mailto:'+data.email">{{data.email_text}}</a></li>
          <li><button class="bg-transparent hover:bg-transparent hover:underline" @click="isFormVisible = true">Formulaire de contact</button></li>
        </ul>
      </div>
    </div>
    <contact-form 
      v-show="isFormVisible"
      @close="isFormVisible = false"/>
  </section>
</template>

<script>
import ContactForm from '~/components/ContactForm.vue'

export default {
  components: {
    ContactForm
  },
  data() {
    return {
      isFormVisible: false,
    }
  },
  props: ['data'],
}
</script>

<style lang="scss" scoped>
.sidebar {
  height: 100vh; /* Fallback for browsers that do not support Custom Properties */
  height: calc(var(--vh, 1vh) * 100);

  @media (min-width: 640px) and (max-width: 1024px) {
    height: auto;
  }

  @media (min-width: 1024px) {
    position: sticky;
    min-width: 300px;
    top: 0;
    min-height: 640px;
  }
}

.text {
  p {
    margin-bottom: 0.5rem;
  }
}

$heightbubble: 30px;

.chat-bubble{
  height: $heightbubble;
  width: 10px;
  background: #e5e5e5;
  position: absolute;
  right: -25px;
  bottom: -5px;
  &:before{
    content: '';
    height: $heightbubble;
    width: $heightbubble;
    left: calc(-1 * #{$heightbubble} / 2);
    position: absolute;
    display: block;
    background: #e5e5e5;
    border-radius: 50%;
    z-index: 1;
  }
  &:after{
    content: '';
    height: $heightbubble;
    width: $heightbubble;
    right: calc(-1 * #{$heightbubble} / 2);
    position: absolute;
    display: block;
    background: #e5e5e5;
    border-radius: 50%;
    z-index: 1;
  }
  .tail{
    height: 10px;
    width: 10px;
    background: #e5e5e5;
    position: absolute;
    left: -16px;
    top: 0px;
    border-radius: 50%;
    &:before{
      height: 5px;
      width: 5px;
      background: #e5e5e5;
      content: '';
      display: block;
      border-radius: 50%;
      position: absolute;
      left: -3px;
      top: -4px;
    }
  }
  .loading{
    position: absolute;
    z-index: 10;
    left:-5px;
    top: 12px;
    width: 50px;
    .dot{
      height: 5px;
      width: 5px;
      border-radius: 50%;
      background: #c1c1c1;
      display: block;
      float: left;
      margin: 0 0 0 2px;
      &:first-child{
        margin: 0;
      }
      &.one{
        animation: cycleOne 1s ease-in-out infinite;
        animation-direction: normal;
      }
      &.two{
        animation: cycleTwo 1s ease-in-out infinite;
        animation-direction: normal;
      }
      &.three{
        animation: cycleThree 1s ease-in-out infinite;
        animation-direction: normal;
      }
    }
  }
}

@keyframes cycleOne{
  0%{
    background: rgba(150, 150, 150, 0.4);
  }
  33.333%{
    background: rgba(150, 150, 150, 1);
  }
  66.6667%{
    background: rgba(150, 150, 150, 0.4);
  }
  100%{
    background: rgba(150, 150, 150, 0.4);
  }
}

@keyframes cycleTwo{
  0%{
    background: rgba(150, 150, 150, 0.4);
  }
  33.333%{
    background: rgba(150, 150, 150, 0.4);
  }
  66.6667%{
    background: rgba(150, 150, 150, 1);
  }
  100%{
    background: rgba(150, 150, 150, 0.4);
  }
}

@keyframes cycleThree{
  0%{
    background: rgba(150, 150, 150, 0.4);
  }
  33.333%{
    background: rgba(150, 150, 150, 0.4);
  }
  66.6667%{
    background: rgba(150, 150, 150, 0.4);
  }
  100%{
    background: rgba(150, 150, 150, 1);
  }
}
</style>