<template>
  <v-container>
    <h1>Cadastro de Categorias</h1>
    <hr>
    <v-form v-model="valid">
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
            :rules="nomeRules"
            required
            solo
            />
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-btn
      outlined
      to="/categorias"
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
      valid: false,
      categoria: {
        id: null,
        nome: null
      },
      nomeRules: [
        v => !!v || `Informe o nome da categoria`
      ],
    }
  },

  methods: {
    async cadastrar () {
      try {
        if (!this.valid) {
          return this.$toast.warning('O formulário de cadastro não é válido')
        }
        let categoria = {
          nome: this.categoria.nome
        }
        let response = await this.$axios.$post('http://localhost:3333/categorias', categoria);
        this.$toast.success(`Categoria ${categoria.nome} cadastrado com sucesso`);
        this.$router.push('/categorias');
        console.log(response);
      } catch(error) {
        this.$toast.error('Ocorreu um erro ao registrar o formulário');
      }
    }
  }
}
</script>