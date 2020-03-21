<template>
  <div>
      <h1 v-text="title"></h1>

      <form @submit.prevent="onSubmit">
          <div :class="{'has-error': error}">
                <input type="text" placeholder="CEP:" v-model="cep">
                <button type="submit">Buscar</button>

                <div v-if="error" v-text="error"></div>
          </div>
      </form>     

      <div v-if="address.cep">
          <p>Cep: {{ address.cep }}</p>
          <p>logradouro: {{ address.logradouro }}</p>
          <p>complemento: {{ address.complemento }}</p>
          <p>bairro: {{ address.bairro }}</p>
          <p>localidade: {{ address.localidade }}</p>
      </div>
  </div>
</template>


<script>
import axios from 'axios'

export default {
  data () {
      return {
          title: 'Busca CEP',
          cep: '',
          address: {},
          preloader: false,
          error: ''
      }
  },
  methods: {
      onSubmit () {
          this.preloader = true
          this.reset()

          axios.get(`https://viacep.com.br/ws/${this.cep}/json/`)
                    .then(response => {
                        console.log(response.data)

                        if (response.data.erro)
                            this.error = 'Cep Inválido!'
                        else
                            this.address = response.data
                    })
                    .catch(error => {
                        console.log(error)
                        this.error = '404'
                    })
                    .finally(() => this.preloader = false)
      },
      reset () {
          this.error = ''
          this.address = {}
      }
  }
}
</script>


<style scoped>
.preloader{max-width: 100px;}
.has-error{color: rgb(184, 33, 33);}
.has-error input{border: 1px solid rgb(184, 33, 33);}
</style>
