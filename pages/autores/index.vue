<template>
  <v-container>
    <h1>Autores</h1>
    <hr>
    <v-container>
      <v-row>
        <v-col>
          <v-btn
          solo
          @click="getAutores"
          >
            Pesquisar
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
          solo
          to="/autores/cadastro"
          >
            Cadastrar
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="livros"
        :items-per-page="10"
        class="elevation-1"
      >
      </v-data-table>
    </v-container>
  </v-container>
</template>

<script>
export default {
  name: 'ConsultaAutoresPage',

  data () {
    return {
      headers: [
        {
          text: 'Código',
          align: 'center',
          sortable: false,
          value: 'id',
        },
        {
          text: 'Nome',
          align: 'center',
          sortable: false,
          value: 'nome',
        },
        {
          text: 'Email',
          align: 'center',
          sortable: false,
          value: 'email',
        }
      ],
      livros: []
    }
  },

  created() {
    this.getAutores()
  },

  methods: {
    async getAutores () {
      this.livros = await this.$axios.$get('http://localhost:3333/autores')
    }
  }
}
</script>

<style>

</style>