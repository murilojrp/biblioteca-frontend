<template>
  <v-container>
    <h1>Consulta de Categorias</h1>
    <hr>
    <v-container>
        <v-row>
            <v-col>
            <v-btn
        outlined
        @click="getCategorias"
    >
        Pesquisar
    </v-btn>
    </v-col>
    <v-col>
          <v-btn
            outlined
            to="/categorias/cadastro"
          >
            Cadastrar
          </v-btn>
        </v-col>
        </v-row>
    </v-container>
    <v-container>
        <v-data-table
         :headers="headers"
         :items="categorias"
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
    name: 'ConsultaCategoriasPage',

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
                { text: "", value: "actions" }
            ],
            categorias: []
        }
    },

    created () {
        this.getCategorias()
    },

    methods: {
        async getCategorias () {
            this.categorias = await this.$axios.$get('http://localhost:3333/categorias');
        },

        async deleteItem (categoria) {
            try {
            if (confirm(`Deseja deletar a categoria ID:${categoria.id}-${categoria.nome}?`)) {
                let response = await this.$axios.$post('http://localhost:3333/categorias/deletar', { id: categoria.id });
                this.$toast.success(`Categoria ID:${categoria.id}-${categoria.nome} deletada com sucesso!`)
                this.getCategorias();
            } 
            } catch (error) {
            this.$toast.error('Ocorreu um erro ao atender a requisição. Contate o Gabriel.')
        }
     },
    async editItem (categoria) {
      this.$router.push({
        name: 'categorias-cadastro',
        params: { id: categoria.id }
      });
    }
  }
}
</script>