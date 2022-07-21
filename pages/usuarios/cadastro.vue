<template>
  <v-container>
    <h1>Cadastro de Usuários</h1>
    <hr>
    <v-form>
      <v-container>
        <v-row>
          <v-col>
            <v-text-field
            v-model="usuario.id"
            placeholder="Código"
            disabled
            solo
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.nome"
              placeholder="Nome"
              solo
            />
          </v-col>
        </v-row>
        <v-row>
          <v-text-field
            v-model="usuario.email"
            placeholder="Email"
            solo
          >
          </v-text-field>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.telefone"
              placeholder="Telefone"
              solo
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-
              v-model="usuario.cpfcnpj"
              placeholder="CPF/CNPJ"
              solo
            >
            </v-text->
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-btn
      solo
      to="/usuarios"
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
  name: 'CadastroUsuariosPage',

  data () {
    return {
      usuario: {
        id: null,
        nome: null,
        email: null,
        telefone: null,
        cpfcnpj: null,
      }
    }
  },

  methods: {
    async cadastrar () {
      let usuario = {
        nome: this.usuario.nome,
        email: this.usuario.email,
        telefone: this.usuario.telefone,
        cpfcpnj: this.usuario.cpfcnpj,
      }
      let response = await this.$axios.$post('http://localhost:3333/usuarios', usuario);
      console.log(response);
    }
  },

  created () {
    this.getUsuarios()
  },

  methods: {
    async getUsuarios () {
      this.usuarios = await this.$axios.$get('http://localhost:3333/usuarios');
    }
  }
}
</script>