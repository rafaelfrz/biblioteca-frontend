<template>
  <v-container>
    <h1>Cadastro de Categorias</h1>
    <hr>
    <v-form>
      <v-container>
        <v-row>
          <v-col>
            <v-text-field
            v-model="categoria.id"
            placeholder="Código"
            disabled
            solo
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
            v-model="categoria.nome"
            placeholder="Nome"
            solo
            />
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-btn
      outlined
      @click="cancelar"
    >
      Cancelar
    </v-btn>
    <v-btn
      outlined
      @click="cadastrar"
      >
        Cadastrar
    </v-btn>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroCategoriasPage',

  data () {
    return {
      categoria: {
        id: null,
        nome: null
      }
    }
  },

  methods: {
    async cadastrar () {
      let categoria = {
        nome: this.categoria.nome
      }
      let response = await this.$axios.$post('http://localhost:3333/categorias', categoria);
      console.log(response);
    }
  }
}
</script>