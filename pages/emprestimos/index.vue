<template>
  <v-container style="background-color: red;">
    <h1> Emprestimos </h1>
    <hr>
    <v-container>
      <v-row>
        <v-col>
          <v-btn
            solo
            @click="getEmprestimos"
          >
            Pesquisar
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            solo
            to="/emprestimos/cadastro"
          >
            Cadastrar
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container style="background-color: blue;">
      <v-data-table
        :headers="headers"
        :items="emprestimos"
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
    name: 'ConsultaEmprestimosPage',

    data() {
        return {
            headers: [{
                text: 'Código',
                align: 'center',
                sortable: false,
                value: 'id',
            },
            {
                text: 'Usuário',
                align: 'center',
                sortable: false,
                value: 'usuario',
            },
            {
                text: 'Prazo',
                align: 'center',
                sortable: false,
                value: 'prazo',
            },
            {
                text: 'Situacão',
                align: 'center',
                sortable: false,
                value: 'id',
            },
            { text: "", value: "actions" }
            ],
            emprestimos: [],
        }
    },

    created() {
        this.getEmprestimos()
    },

  methods: {
      async getEmprestimos() {
          this.emprestimos = await this.$axios.$get('http://localhost:3333/emprestimos')
      },

      async deletar(emprestimo) {
      try {
        if (confirm(`Deseja deletar o emprestimo ID ${emprestimo.id}`)) {
          let response = await this.$axios.$post('http://localhost:3333/livros/deletar', { id: emprestimo.id });
          this.$toast.success(response.message);
          this.getEmprestimos();
        }
      } catch(error) {
        console.log(error.message);
        this.$toast.error('Ocorreu um erro ao atender a requisição. Contate o administrador/suporte');
      }
    },

    async editar(emprestimo){
      this.$router.push({
        name: 'emprestimos-cadastro',
        params: { id: emprestimo.id }
      });
    }
  }
}
</script>

<style>

</style>