<template>
  <v-container>
    <h1>Categorias</h1>
    <hr>
    <v-container>
      <v-row>
        <v-col>
          <v-btn
          solo
          @click="getCategorias"
        >
          Pesquisar
          </v-btn>
        </v-col>
        <v-col
        cols="2"
        >
          <v-btn
          solo
          to="/categorias/cadastro"
        >
          Cadastrar
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="categorias"
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
  name: 'ConsultaCategoriasPage',

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
        { text: "", value: "actions" }
      ],
      categorias: []
    }
  },

  created() {
    this.getCategorias()
  },
  
  methods: {
    async getCategorias () {
      this.categorias = await this.$axios.$get('http://localhost:3333/categorias');
    },

    async deletar(categoria) {
      try {
        if(confirm(`Deseja deletar a categoria id ${categoria.id} - ${categoria.nome}?`)) {
          let response = await this.$axios.$post('http://localhost:3333/categorias/deletar', { id: categoria.id });
          this.$toast.success(response.message);
          this.getCategorias();
        } 
      } catch(error) {
        console.log(error.message);
        this.$toast.error('Ocorreu um erro ao atender a requisição. Contate o administrador/suporte');
      }
    },
    
    async editar(categoria){
      this.$router.push({
        name: 'categorias-cadastro',
        params: { id: categoria.id }
      });
    }
  }
}
</script>