<template>
  <v-container>
    <h1>Busca de Livros</h1>
    <hr>
    <v-row>
        <v-col>
            <v-autocomplete
                v-model="livro.id"
                :items="livros"
                outlined
                label="Livros"
                item-text="titulo"
                item-value="id"
                style="margin-top:2%"
            ></v-autocomplete>
        </v-col>
    </v-row>
    <v-row>
        <v-col>
        <v-btn
            outlined
            @click="buscar"
        >
            Buscar
        </v-btn>
        </v-col>
        <v-col>
        <v-btn
                outlined
                to="/livros"
            >
                Voltar
            </v-btn>
            </v-col>
    </v-row>
    <v-row>
        <v-container>
            <v-card
                elevation="2"
                style="margin-top:5%"
            > <v-card-title>
               {{response.message}}    
            </v-card-title>
                <v-card-text v-if="response.emprestimos">
                    Número do empréstimo: {{response.emprestimos[0]?.id}}
                    <br>
                    Prazo: {{response.emprestimos[0]?.prazo}}
                    <br>
                    Data do Empréstimo: {{response.emprestimos[0]?.created_at.substr(0,10)}}
                    <br>
                    Código do usuário: {{response.emprestimos[0]?.idUsuario}}
                </v-card-text>
            </v-card>
        </v-container>
    </v-row>
  </v-container>
</template>

<script>
export default {
    name: 'BuscaLivrosPage',

    data () {
        return {
            livro: {
                id: null
            }, 
            livros: [],
            response: {},
            usuario: {}
        }
    },
    
    created () {
        this.getLivros();
    },

    methods: {
        async getLivros () {
            this.livros = await this.$axios.$get('http://localhost:3333/livros');
        },
        async buscar () {
            this.response = await this.$axios.$post('http://localhost:3333/emprestimos/validar', {idLivro: this.livro.id})
            }
        }
}
</script>