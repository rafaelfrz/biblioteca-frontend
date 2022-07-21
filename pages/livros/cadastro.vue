<template>
  <v-container>
    <h1>Cadastro de Livros</h1>
    <hr>
    <v-form>
      <v-container>
        <v-row>
          <v-col>
            <v-text-field
            v-model="livro.id"
            placeholder="Código"
            disabled
            solo
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
            v-model="livro.titulo"
            placeholder="Título"
            solo
            />
          </v-col>
        </v-row>
        <v-row>
          <v-text-field
          v-model="livro.sinopse"
          placeholder="Sinopse"
          solo
          >
          </v-text-field>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
            v-model="livro.idCategoria"
            placeholder="Categoria"
            solo
            >
            </v-text-field>
          </v-col>
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
      @click="cadastrar"¨
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
      livro: {
        id: null,
        titulo: null,
        sinopse: null,
        emprestado: null,
        idCategoria: null,
        idAutor: null,
      }
    }
  },

  methods: {
    async cadastrar () {
      let livro = {
        titulo: this.livro.titulo,
        sinopse: this.livro.sinopse,
        emprestado: this.livro.emprestado,
        idCategoria: this.livro.idCategoria,
        idAutor: this.livro.idAutor,
      }
      let response = await this.$axios.$post('http://localhost:3333/livros', livro);
      console.log(response);
    }
  },

  created () {
    this.getLivros()
  },

  methods: {
    async getLivros () {
      this.livros = await this.$axios.$get('http://localhost:3332/livros');
    }
  }
}
</script>