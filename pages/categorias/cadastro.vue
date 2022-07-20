<template>
  <v-container>
    <h1>Cadastro de Categorias</h1>
    <hr>
    <v-form>
    <v-container>
        <v-row>
            <v-col
            cols="2"
            >
                <v-text-field
                    v-model="categoria.id"
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
                    v-model="categoria.nome"
                    placeholder="Nome"
                    label="Nome"
                    outlined
                >
                </v-text-field>
            </v-col>
        </v-row>
    </v-container>
    </v-form>
    <v-btn
        outlined
        to="/categorias"
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
    name: 'CadastroCategoriasPage',

    data () {
        return {
            categoria: {
                id: null,
                nome: null
            }
        }
    },

    methods: {
        async cadastrar () {
            let categoria = {
                nome: this.categoria.nome
            }
            let response = await this.$axios.$post('http://localhost:3333/categorias', categoria);
            console.log(response); 
        }
    }
}
</script>