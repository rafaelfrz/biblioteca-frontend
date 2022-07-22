<template>
  <v-container>
    <h1> Livros </h1>
    <hr>
    <v-container>
      <v-row>
        <v-col>
          <v-btn
            solo
            @click="getLivros"
          >
            Pesquisar
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            solo
            to="/livros/cadastro"
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
        <template v-slot:item.actions="{ item }">
          <v-icon
            small
            class="mr-2"
            @click="editar(item)"
          >
            mdi-pencil
          </v-icon>
          <v-icon
            small
            @click="deletar(item)"
          >
            mdi-delete
          </v-icon>
        </template>
      </v-data-table>
    </v-container>
  </v-container>
</template>

<script>
export default {
  name: 'ConsultaLivrosPage',

  data () {
    return {
      headers: [
        {
          text: 'Código',
          align: 'center',
          sortable: true,
          value: 'id',
        },
        {
          text: 'Título',
          align: 'center',
          sortable: false,
          value: 'titulo',
        },
        {
          text: 'Sinopse',
          align: 'center',
          sortable: false,
          value: 'sinopse'
        },
        {
          text: 'Categoria',
          align: 'center',
          sortable: true,
          value: 'categoria.nome'
        },
        {
          text: 'Autor',
          align: 'center',
          sortable: true,
          value: 'autor.nome'
        },
        { text: "", value: "actions" }
      ],
      livros: [],
    }
  },

  created() {
    this.getLivros()
  },

  methods: {
    async getLivros () {
      this.livros = await this.$axios.$get('http://localhost:3333/livros')
    },

    async deletar(livro) {
      try {
        if (confirm(`Deseja deletar o livro ID ${livro.id} - ${livro.nome}`)) {
          let response = await this.$axios.$post('http://localhost:3333/livros/deletar', { id: livro.id });
          this.$toast.success(response.message);
          this.getLivros();
        }
      } catch(error) {
        console.log(error.message);
        this.$toast.error('Ocorreu um erro ao atender a requisição. Contate o administrador/suporte');
      }
    },

    async editar(livro){
      this.$router.push({
        name: 'livros-cadastro',
        params: { id: livro.id }
      });
    }
  }
}

</script>