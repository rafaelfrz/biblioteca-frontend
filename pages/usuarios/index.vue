<template>
  <v-container>
    <h1>Usuários</h1>
    <hr>
    <v-container>
      <v-row>
        <v-col>
          <v-btn
            solo
            @click="getUsuarios"
          >
            Pesquisar
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            solo
            to="/usuarios/cadastro"
          >
            Cadastrar
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="usuarios"
        :items-per-page="10"
        class="elevation-1"
      >
        <template v-slot:item.actions="{ item }">
          <v-icon
            small
            class="mr-2"
            @click="editItem(item)"
          >
            mdi-pencil
          </v-icon>
          <v-icon
            small
            @click="deletItem(item)"
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
  name: 'ConsultaUsuarioPage',

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
        {
          text: 'Telefone/Celular',
          align: 'center',
          sortable: false,
          value: 'telefone',
        },
        { text: "", value: "actions" }
      ],
      usuarios: [],
    }
  },

  created() {
    this.getUsuarios()
  },

  methods: {
    async getUsuarios () {
      this.usuarios = await this.$axios.$get('http://localhost:3333/usuarios')
    }
  }
}
</script>

<style>

</style>