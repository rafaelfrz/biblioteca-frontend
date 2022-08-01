<template>
    <v-container style="background-color: red; max-height: 90%;">
        <h1>Disponibilidade livros</h1>
        <hr>
        <v-container>
            <v-row>
                <v-col cols="11" style="vertical-align: middle">
                    <v-autocomplete v-model="idLivro" :items="livros" placeholder="Selecione um livro" solo clearable item-value="id"
                        item-text="titulo">
                    </v-autocomplete>
                </v-col>
                <v-col>
                    <v-btn x-small style="width: 50px; height: 50px;" @click="consulta">
                        <v-icon>
                            mdi-magnify
                        </v-icon>
                    </v-btn>
                </v-col>
            </v-row>
            <v-container style="background-color: blue;">
                <v-row style="padding: 10px;">
                    <v-col style="background-color: yellow; margin-right: 10px;" cols="4">
                        <p style="color: black; font-weight: bolder; text-align: center;">CAPA</p>
                    </v-col>
                    <v-col>
                        <v-container style="background-color: green;">
                            <v-row>
                                <v-col>
                                    Título: {{ livro.titulo }}
                                </v-col>
                            </v-row>
                            <v-row>
                                <v-col>
                                    Categoria: {{ this.categoria }}
                                </v-col>
                            </v-row>
                            <v-row>
                                <v-col>
                                    Sinopse: {{ livro.sinopse }}
                                </v-col>
                            </v-row>
                            <v-row>
                                <v-col>
                                    Autor: {{ this.autor }}
                                </v-col>
                            </v-row>
                            <v-row>
                                <v-col>
                                    Situação: {{ situacao }}
                                </v-col>
                            </v-row>
                        </v-container>
                    </v-col>
                </v-row>
            </v-container>
        </v-container>
    </v-container>
</template>

<script>
`export default {
    name: 'ConsultaLivrosPage',
    data() {
        return {
            idLivro: null,
            livro: [],
            autor: null,
            categoria: null,
            situacao: null,
            livros: [],
            rule: [
                v => !!v || 'Campo obrigatório'
            ],
        }
    },

    created() {
        this.getLivros()
    },

    methods: {
        async getLivros() {
            this.livros = await this.$axios.$get('http://localhost:3333/livros')
        },

        async consulta() {
            this.livros.forEach((livro) => {
                if (livro.id == this.idLivro) {
                    this.livro = livro
                    this.autor = livro.autor.nome
                    this.categoria = livro.categoria.nome
                }
            });
            this.buscarSituacao()
        },

        async buscarSituacao() {
            let response = await this.$axios.$post('http://localhost:3333/emprestimos/emprestado', { idLivro: this.idLivro })
            this.situacao = response.message
        }
    }
}

</script>

<style>
</style>