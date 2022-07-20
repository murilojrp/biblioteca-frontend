<template>
  <v-container>
    <h1>Consulta de Livros</h1>
    <hr>
    <v-container>
        <v-row>
            <v-col>
            <v-btn
        outlined
        @click="getLivros"
    >
        Pesquisar
    </v-btn>
    </v-col>
    <v-col>
          <v-btn
            outlined
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
        ></v-data-table>
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
                    value: 'sinopse',
                },
                {
                    text: 'Autor',
                    align: 'center',
                    sortable: false,
                    value: 'autor.nome',
                },
                {
                    text: 'Categoria',
                    align: 'center',
                    sortable: false,
                    value: 'categoria.nome',
                }
            ],
            livros: []
        }
    },

    created () {
        this.getLivros()
    },

    methods: {
        async getLivros () {
            this.livros = await this.$axios.$get('http://localhost:3333/livros');
        }
    }

}
</script>