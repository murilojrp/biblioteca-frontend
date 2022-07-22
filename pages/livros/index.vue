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
        <v-col>
          <v-btn
            outlined
            to="/livros/busca"
            color="purple"
          >
            Busca por Código
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
         style="background-color: #350845; border:double"
        >
        <template v-slot:item.actions="{ item }">
            <v-icon
                small
                class="mr-2"
                @click="editItem(item)"
                color="blue"
            >
                mdi-pencil
            </v-icon>
            <v-icon
                small
                @click="deleteItem(item)"
                color="red"
            >
                mdi-delete
            </v-icon>
        </template>
<template v-slot:no-data>
  <v-btn
    color="primary"
    @click="initialize"
  >
    Reset
  </v-btn>
</template>
        </v-data-table>
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
                },
                { text: "", value: "actions" }
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
        },

        async deleteItem (livro) {
            try {
            if (confirm(`Deseja deletar o livro ID:${livro.id}-${livro.titulo}?`)) {
                let response = await this.$axios.$post('http://localhost:3333/livros/deletar', { id: livro.id });
                this.$toast.success(`Livro ID:${livro.id}-${livro.titulo} deletado com sucesso!`)
                this.getLivros();
                } 
            } catch (error) {
            this.$toast.error('Ocorreu um erro ao atender a requisição. Contate o Gabriel.')
        }
    },
    async editItem (livro) {
      this.$router.push({
        name: 'livros-cadastro',
        params: { id: livro.id }
      });
    }        
    }
}
</script>