<template>
    <v-container>
        <h1>Cadastro de Emprestimos</h1>
        <hr>
        <v-form v-model="valid">
            <v-container>
                <v-row>
                    <v-col>
                        <v-text-field v-model="emprestimo.id" placeholder="Código" disabled solo />
                    </v-col>
                </v-row>
                <v-row>
                    <v-col>
                        <v-autocomplete placeholder="Usuário" :rules="nomeRules" required :items="usuarios"
                            item-text="nome" solo></v-autocomplete>
                    </v-col>
                </v-row>
                <v-row>
                    <v-col>
                        <v-autocomplete placeholder="Livros" chips :items="livros" item-text="titulo" deletable-chips
                            multiple solo></v-autocomplete>
                    </v-col>
                </v-row>
                <v-row>
                    <v-col cols="12" sm="6" md="4">
                        <v-dialog ref="dialog" v-model="modal" :return-value.sync="date" persistent width="290px">
                            <template v-slot:activator="{ on, attrs }">
                                <v-text-field v-model="date" label="Picker in dialog" prepend-icon="mdi-calendar"
                                    readonly v-bind="attrs" v-on="on"></v-text-field>
                            </template>
                            <v-date-picker v-model="date" scrollable>
                                <v-spacer></v-spacer>
                                <v-btn text color="primary" @click="modal = false">
                                    Cancel
                                </v-btn>
                                <v-btn text color="primary" @click="$refs.dialog.save(date)">
                                    OK
                                </v-btn>
                            </v-date-picker>
                        </v-dialog>
                    </v-col>

                </v-row>
            </v-container>
        </v-form>
        <v-btn solo to="/emprestimos">
            Cancelar
        </v-btn>
        <v-btn solo @click="persistir">
            Cadastrar
        </v-btn>
    </v-container>
</template>

<script>
export default {
    name: 'CadastroEmprestimoPage',

    data() {
        return {
            valid: false,
            emprestimo: {
                id: null,
                data: null
            },
            livros: [],
            usuarios: null,
        }
    },

    created() {
        if (this.$route?.params?.id) {
            this.getById(this.$route.params.id)
        }
        this.getUsuarios();
        this.getLivros();
    },

    methods: {

        async persistir() {
            try {
                if (!this.valid) {
                    return this.$toast.warning('O formulário de cadastro não é válido')
                };

                let emprestimo = {
                    nome: this.emprestimo.nome
                };

                if (!this.emprestimo.id) {
                    await this.$axios.$post('http://localhost:3333/emprestimos', emprestimo);
                    this.$toast.success(`Categoria ${emprestimo.nome} cadastrado com sucesso`);
                    return this.$router.push('/emprestimos');
                }

                await this.$axios.$post(`http://localhost:3333/emprestimos/${this.emprestimo.id}`, emprestimo)

                this.$toast.success('Cadastro atualizado com sucesso');
                return this.$router.push('/emprestimos');
            } catch (error) {
                this.$toast.error('Ocorreu um erro ao registrar o formulário');
            }
        },

        async getById(id) {
            this.emprestimo = await this.$axios.$get(`http://localhost:3333/emprestimo/${id}`);
        },

        async getUsuarios() {
            this.usuarios = await this.$axios.$get(`http://localhost:3333/usuarios`)
        },

        async getLivros() {
            this.livros = await this.$axios.$get(`http://localhost:3333/livros`)
        }
    }
}
</script>

<style>
</style>