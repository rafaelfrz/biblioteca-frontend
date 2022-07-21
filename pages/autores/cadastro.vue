<template>
  <v-container>
    <h1>Cadastro de Autores</h1>
    <hr>
    <v-form v-model="valid">
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
            :rules="nomeRules"
            required
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
      valid: false,
      autor: {
        id: null,
        nome: null,
        email: null,
      },
      nomeRules: [
        v => !!v || 'Informe o nome do autor'
      ],
    }
  },

  methods: {
    async cadastrar () {
      try {
        if (!this.valid) {
          return this.$toast.warning('O formulário de cadastro não é válido!')
        }
        let autor = {
          nome: this.autor.nome,
          email: this.autor.email,
        }
        let response = await this.$axios.$post('http://localhost:3333/autores', autor)
        this.$toast.success(`Autor ${autor.nome} cadastrado com sucesso`)
        this.$router.push('/autores')
        console.log(response);
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao registrar o formulário');
      }
    }
  }
}
</script>

<style>

</style>