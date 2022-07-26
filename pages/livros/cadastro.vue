<template>
  <v-container style="background-color: red;">
    <h1>Cadastro de Livros</h1>
    <hr>
    <v-form v-model="valid">
      <v-container style="background-color: blue;">
        <v-row>
          <v-col cols="1">
            <v-text-field
              v-model="livro.id"
              placeholder="Código"
              disabled
              solo
            />
          </v-col>
            <v-col cols="8">
              <v-text-field
              v-model="livro.titulo"
              placeholder="Título"
              :rules="tituloRules"
              required
              solo
            />
          </v-col>
          <v-col cols="3">
            <v-text-field
              v-model="livro.idCategoria"
              placeholder="Categoria"
              :rules="idCategoriaRules"
              required
              solo
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="livro.sinopse"
              placeholder="Sinopse"
              solo
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="livro.idAutor"
              placeholder="Autor"
              solo
            >
            </v-text-field>
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-btn
      solo
      to="/livros"
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
  name: 'CadastroLivrosPage',

  data () {
    return {
      valid: false,
      livro: {
        id: null,
        titulo: null,
        sinopse: null,
        emprestado: null,
        idCategoria: null,
        idAutor: null,
      },
      tituloRules: [
        v => !!v || 'Informe o titulo do livro'
      ],
      idCategoriaRules: [
        v => !!v || 'Informe a categoria do livro'
      ]
    }
  },

  created() {
    if(this.$route?.params?.id) {
      this.getById(this.$route.params.id)
    }
  },

  methods: {
    async persistir () {
      try {
        if (!this.valid) {
          return this.$toast.warning('O formulário de cadastro não é válido!')
        };

        let livro = {
          titulo: this.livro.titulo,
          sinopse: this.livro.sinopse,
          emprestado: this.livro.emprestado,
          idCategoria: this.livro.idCategoria,
          idAutor: this.livro.idAutor,
        };

        if(!this.livro.id) {
          await this.$axios.$post('http://localhost:3333/livros', livro);
          this.$toast.success(`Livro ${livro.titulo} cadastrado com sucesso`);
          return this.$router.push('/livros');
        };

        await this.$axios.$post(`http://localhost:3333/livros/${this.livro.id}`, livro)

        this.$toast.success('Cadastro atualizado com sucesso');
        return this.$router.push('/livros');
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao registrar o formulário');
      }
    },

    async getAutores () {
      this.autores = await this.$axios.$get('http://localhost:3333/autores');
    },

    async getCategorias () {
      this.categorias = await this.$axios.$get('http://localhost:3333/categorias');
    },

    async getById(id) {
      this.livro = await this.$axios.$get(`http://localhost:3333/livros/${id}`);
    }
  }
}
</script>