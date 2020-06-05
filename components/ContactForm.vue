<template>
  <transition name="component-fade">
    <div class="modal-backdrop" @click="clickOutside">
      <form 
        class="modal text-left w-11/12 lg:w-1/2" 
        @submit="checkForm" 
        method="post">
        <div class="mb-4">
          <label for="name">Prénom NOM :</label>
          <input v-model="name" type="text" name="name" placeholder="Nom" class="block mt-2 bg-gray-200 rounded w-full py-2 px-3">
        </div>
        <div class="mb-4">
          <label for="mail">Email :</label>
          <input v-model="email" type="email" name="email" placeholder="Email" class="block mt-2 bg-gray-200 rounded w-full py-2 px-3">
        </div>
        <div class="mb-4">
          <label for="msg">Message :</label>
          <textarea rows="4" v-model="message" name="message" placeholder="Message" class="block mt-2 bg-gray-200 rounded w-full py-2 px-3"></textarea>
        </div>
        <div class="mb-4 text-center">
          <input type="text" name="website" v-model="website" class="hidden opacity-0 z-0" tabindex="-1" autocomplete="off">
          <input type="submit" value="Envoyer" :class="{ 'cursor-not-allowed opacity-50': loading }" class="cursor-pointer bg-wonder-green text-white font-bold py-2 px-4 rounded">
          <button class="cursor-pointer bg-gray-700 text-white py-2 px-4 hover:bg-gray-700 rounded" @click.prevent="close">Annuler</button>
        </div>
        <div v-if="errors.length" class="text-center text-red-500">
          <b>Veuillez remplir tous les champs</b>
        </div>
        <div v-if="success" class="text-green-500">
          <b>Votre message a bien été envoyé</b>
        </div>
      </form>
    </div>
  </transition>
</template>

<script>
import axios from 'axios'

export default {
  data: function () {
    return {
      errors: [],
      name: null,
      email: null,
      message: null,
      loading: false,
      success: false,
      website: null,
    }
  },

  methods: {
    clickOutside(e) {
      if (e.target.classList.contains('modal-backdrop')) {
        this.close();
      }
    },
    checkForm: function (e) {
      this.errors = []
      this.success = false

      if (!this.name) {
        this.errors.push("Name required")
      }
      if (!this.email) {
        this.errors.push('Email required')
      } else if (!this.validEmail(this.email)) {
        this.errors.push('Valid email required')
      }
      if (!this.message) {
        this.errors.push("Message required")
      }

      if (!this.errors.length) {
        this.submitForm()
      }

      e.preventDefault()
    },

    submitForm: function () {
      this.loading = true

      axios.post(process.env.contactUrl,
      JSON.stringify({
          form: {
            name: this.name,
            email: this.email,
            message: this.message,
            website: this.website,
          }
        }),
      {
        headers: { 'Content-Type': 'application/json' }
      })
      .then(({ data }) => {
        this.loading = false

        if(data.error){
          this.errors.push(data.error)
        } else if(data.name && data.email && data.message) {
          this.name = this.email = this.message = null
          this.success = true
        }
      }).catch(error => {
        this.loading = false

        this.errors.push('An error occured, please try again later')
      })
    },

    validEmail: function (email) {
      let re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
      return re.test(email)
    },

    close() {
      this.$emit('close');
    }
  }
}
</script>

<style lang="scss" scoped>
.modal-backdrop {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;

  .modal {
    background: #FFFFFF;
    box-shadow: 2px 2px 10px 1px gray;
    overflow-x: auto;
    display: flex;
    flex-direction: column;
    color: black;

    @apply p-5;
  }
}
</style>