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
        :items="autores"
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
        },
        { text: "", value: "actions" }
      ],
      autores: []
    }
  },

  created() {
    this.getAutores()
  },

  methods: {
    async getAutores () {
      this.autores = await this.$axios.$get('http://localhost:3333/autores')
    },
    
    async deletar(autor) {
      try {
        if (confirm(`Deseja deletar o autor id ${autor.id} - ${autor.nome}?`)) {
          let response = await this.$axios.$post('http://localhost:3333/autores/deletar', { id: autor.id})
          this.$toast.success(response.message);
          this.getAutores();
        }
      } catch(error) {
        console.log(error.message);
        this.$toast.error('Ocorreu um erro ao atender a requisição. Contate o administrador/suporte');
      }
    },

    async editar(autor){
      this.$router.push({
        name: 'autores-cadastro',
        params: { id: autor.id }
      });
    }
  }
}
</script>