<template>
  <v-container>
    <h1>Consulta de Autores</h1>
    <hr>
    <v-container>
        <v-row>
            <v-col>
            <v-btn
        outlined
        @click="getAutores"
    >
        Pesquisar
    </v-btn>
    </v-col>
    <v-col>
          <v-btn
            outlined
            to="/autores/cadastro"
          >
            Cadastrar
          </v-btn>
        </v-col>
        </v-row>
    </v-container>
    <v-container>
        <v-data-table
         :headers="headers"
         :items="autores"
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
    name: 'ConsultaAutoresPage',

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
                {
                    text: 'E-mail',
                    align: 'center',
                    sortable: false,
                    value: 'email',
                },
                { text: "", value: "actions" }
            ],
            autores: []
        }
    },

    created () {
        this.getAutores()
    },

    methods: {
        async getAutores () {
            this.autores = await this.$axios.$get('http://localhost:3333/autores');
        },

        async deleteItem (autor) {
            try {
            if (confirm(`Deseja deletar o autor ID:${autor.id}-${autor.nome}?`)) {
                let response = await this.$axios.$post('http://localhost:3333/autores/deletar', { id: autor.id });
                this.$toast.success(`Autor ID:${autor.id}-${autor.nome} deletado com sucesso!`)
                this.getAutores();
            } 
            } catch (error) {
            this.$toast.error('Ocorreu um erro ao atender a requisição. Contate o Gabriel.')
        }
     },
    async editItem (autor) {
      this.$router.push({
        name: 'autores-cadastro',
        params: { id: autor.id }
      });
    }
  }

}
</script>