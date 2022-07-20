<template>
  <v-container>
    <h1>Cadastro de Autores</h1>
    <hr>
    <v-form>
      <v-container>
        <v-row>
          <v-col>
            <v-text-field
            v-model="autor.id"
            placeholder="Código"
            disabled
            solo
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
            v-model="autor.nome"
            placeholder="Nome"
            solo
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
            v-model="autor.email"
            placeholder="Email"
            solo
            />
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-btn
      solo
      to="/autores"
    >
      Cancelar
    </v-btn>
    <v-btn
      solo
      @click="cadastrar"
    >
      Cadastrar
    </v-btn>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroAutorPage',

  data () {
    return {
      autor: {
        id: null,
        nome: null,
        email: null,
      } 
    }
  },

  methods: {
    async cadastrar () {
      let autor = {
        nome: this.autor.nome,
        email: this.autor.email,
      }
      let response = await this.$axios.$post('http://localhost:3333/autores', autor)
      console.log(response);
    }
  }
}
</script>

<style>

</style>