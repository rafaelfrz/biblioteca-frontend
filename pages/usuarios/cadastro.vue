<template>
  <v-container>
    <h1>Cadastro de Usuários</h1>
    <hr>
    <v-form v-model="valid">
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
              :rules="nomeRules"
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
            <v-text-field
              v-model="usuario.cpfcnpj"
              placeholder="CPF/CNPJ"
              :rules="cpfcnpjRules"
              solo
            >
            </v-text-field>
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
      @click="persistir "
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
      valid: false,
      usuario: {
        id: null,
        nome: null,
        email: null,
        telefone: null,
        cpfcnpj: null,
      },
      nomeRules: [
        v => !!v || 'Informe o nome da categoria'
      ],
      cpfcnpjRules: [
        v => !!v || 'Informe o CPF/CNPJ'
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
        if(!this.valid) {
          return this.$toast.warning('O formulário de cadastro não é válido')
        }
        let usuario = {
          nome: this.usuario.nome,
          email: this.usuario.email,
          telefone: this.usuario.telefone,
          cpfcnpj: this.usuario.cpfcnpj,
        }

        if (!this.usuario.id) {
          let response = await this.$axios.$post('http://localhost:3333/usuarios', usuario);
          this.$toast.success(`Usuário ${usuario.nome} cadastrado com sucesso`);
          return this.$router.push('/usuarios');
          console.log(response);
        }

        await this.$axios.$post(`http://localhost:3333/usuarios/${this.usuario.id}`, usuario);

        this.$toast.success('Cadastro atualizado com sucesso');
        return this.$router.push('/usuarios')
      } catch(error) {
        this.$toast.error('Ocorreu um erro ao registrar o formulário');
      }
    },

    async getById(id) {
      this.usuario = await this.$axios.$get(`http://localhost:3333/usuarios/${id}`);
    }
  }
}
</script>