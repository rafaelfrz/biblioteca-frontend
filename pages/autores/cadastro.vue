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
      @click="persistir"
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

  created() {
    if (this.$route?.params?.id) {
      this.getById(this.$route.params.id)
    }
  },

  methods: {
    async persistir () {
      try {
        if (!this.valid) {
          return this.$toast.warning('O formulário de cadastro não é válido!')
        }

        let autor = {
          nome: this.autor.nome,
          email: this.autor.email,
        }

        if(!this.autor.id) {
          await this.$axios.$post('http://localhost:3333/autores', autor)
          this.$toast.success(`Autor ${autor.nome} cadastrado com sucesso`)
          return this.$router.push('/autores')
        }

        await this.$axios.$post(`http://localhost:3333/${this.autor.id}`, autor)
  
        this.$toast.success(`Cadastro atualizado com sucesso`)
        return this.$router.push('/autores')
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao registrar o formulário');
      }
    },

    async getById(id) {
      this.autor = await this.$axios.$get(`http://localhost:3333/autores/${id}`)
    }
  }
}
</script>