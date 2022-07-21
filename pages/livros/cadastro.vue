<template>
  <v-container>
    <h1>Cadastro de Livros</h1>
    <hr>
    <v-form>
    <v-container>
        <v-row>
            <v-col
            cols="2"
            >
                <v-text-field
                    v-model="livro.id"
                    placeholder="Código"
                    label="Código"
                    disabled
                    outlined
                >
                </v-text-field>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <v-text-field
                    v-model="livro.titulo"
                    placeholder="Título"
                    label="Título"
                    outlined
                >
                </v-text-field>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <v-textarea
                    v-model="livro.sinopse"
                    placeholder="Sinopse Do Livro"
                    label="Sinopse Do Livro"
                    outlined
                >
                </v-textarea>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <v-autocomplete
              v-model="livro.idAutor"
              :items="autores"
              outlined
              label="Autor"
              item-text="nome"
              item-value="id"
            ></v-autocomplete>
            </v-col>
            <v-col>
                <v-autocomplete
              v-model="livro.idCategoria"
              :items="categorias"
              outlined
              label="Categoria"
              item-text="nome"
              item-value="id"
            ></v-autocomplete>
            </v-col>
        </v-row>
    </v-container>
    </v-form>
    <v-btn
        outlined
        to="/livros"
    >
    Cancelar
    </v-btn>
    <v-btn
        outlined
        @click="cadastrar"
    >
    Cadastrar
    </v-btn>
  </v-container>
</template>

<script>
export default {
    name: 'CadastroLivrosPage',

    data () {
        return {
            livro: {
                id: null,
                titulo: null,
                sinopse: null,
                idAutor: null,
                idCategoria: null
            },
            categorias: [],
            autores: []
        }
    },

    created () {
        this.getAutores ();
        this.getCategorias ();
    },

    methods: {
        async cadastrar () {
            try {
                let livro = {
                titulo: this.livro.titulo,
                idAutor: this.livro.idAutor,
                idCategoria: this.livro.idCategoria,
                sinopse: this.livro.sinopse
            };
                let response = await this.$axios.$post('http://localhost:3333/livros', livro);
                this.$router.push('/livros')
                this.$toast.success(`Livro ID ${response.id} cadastrado com sucesso!`);
            } catch (error) {
                this.$toast.error('Impossível concluir a operação, contate o suporte.')
            }
        },

            async getAutores () {
            this.autores = await this.$axios.$get('http://localhost:3333/autores');
        },

            async getCategorias () {
            this.categorias = await this.$axios.$get('http://localhost:3333/categorias');
        }
    }
}
</script>