<template>
  <v-container>
    <h1>Consulta de Usuários</h1>
    <hr>
    <v-container>
        <v-row>
            <v-col>
            <v-btn
        outlined
        @click="getUsuarios"
    >
        Pesquisar
    </v-btn>
    </v-col>
    <v-col>
          <v-btn
            outlined
            to="/usuarios/cadastro"
          >
            Cadastrar
          </v-btn>
        </v-col>
        </v-row>
    </v-container>
    <v-container>
        <v-data-table
         :headers="headers"
         :items="usuarios"
         :items-per-page="10"
         class="elevation-1"
         style="background-color: #93c9a6; border:double"
        >
        <template v-slot:item.actions="{ item }">
            <v-icon
                small
                class="mr-2"
                @click="editItem(item)"
            >
                mdi-pencil
            </v-icon>
            <v-icon
                small
                @click="deleteItem(item)"
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
    name: 'ConsultaUsuariosPage',

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
                    text: 'CPF/CNPJ',
                    align: 'center',
                    sortable: false,
                    value: 'cpfcnpj',
                },
                {
                    text: 'E-mail',
                    align: 'center',
                    sortable: false,
                    value: 'email',
                },
                {
                    text: 'Telefone',
                    align: 'center',
                    sortable: false,
                    value: 'telefone',
                },
                { text: "", value: "actions" }
            ],
            usuarios: []
        }
    },

    created () {
        this.getUsuarios()
    },

    methods: {
        async getUsuarios () {
            this.usuarios = await this.$axios.$get('http://localhost:3333/usuarios');
        },

        async deleteItem (usuario) {
            if (confirm(`Deseja deletar o usuário ID:${usuario.id}-${usuario.nome}?`)) {
                let response = await this.$axios.$post('http://localhost:3333/usuarios/deletar', { id: usuario.id });
                this.$toast.success(`Usuário ID:${usuario.id}-${usuario.nome} deletado com sucesso!`)
                this.getUsuarios();
            }
        }
    }

}
</script>