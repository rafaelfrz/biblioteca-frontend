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
      solo
      to="/categorias"
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

  created() {
    if (this.$route?.params?.id) {
      this.getById(this.$route.params.id)
    }
  },

  methods: {
    async persistir () {
      try {
        if (!this.valid) {
          return this.$toast.warning('O formulário de cadastro não é válido')
        };
        
        let categoria = {
          nome: this.categoria.nome
        };

        if (!this.categoria.id) {
          await this.$axios.$post('http://localhost:3333/categorias', categoria);
          this.$toast.success(`Categoria ${categoria.nome} cadastrado com sucesso`);
          return this.$router.push('/categorias');
        }

        await this.$axios.$post(`http://localhost:3333/categorias/${this.categoria.id}`, categoria)
        
        this.$toast.success('Cadastro atualizado com sucesso');
        return this.$router.push('/categorias');
      } catch(error) {
        this.$toast.error('Ocorreu um erro ao registrar o formulário');
      }
    },

    async getById(id) {
      this.categoria = await this.$axios.$get(`http://localhost:3333/categorias/${id}`);
    }
  }
}
</script>