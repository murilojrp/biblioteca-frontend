<template>
  <v-container>
    <h1>Consulta de Empréstimos</h1>
    <hr>
    <v-container>
        <v-row>
            <v-col>
            <v-btn
        outlined
        @click="getEmprestimos"
    >
        Pesquisar
    </v-btn>
    </v-col>
    <v-col>
          <v-btn
            outlined
            to="/emprestimos/cadastro"
          >
            Cadastrar
          </v-btn>
        </v-col>
        </v-row>
    </v-container>
    <v-container>
        <v-data-table
         :headers="headers"
         :items="emprestimos"
         :items-per-page="10"
         class="elevation-1"
         style="background-color: #350845; border:double"
        >
        <template v-slot:item.actions="{ item }">
            <v-icon
                small
                class="mr-2"
                @click="endEmprestimo(item)"
                color="blue"
            >
                mdi-keyboard-return
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
    name: 'ConsultaEmprestimosPage',

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
                    text: 'Prazo',
                    align: 'center',
                    sortable: false,
                    value: 'prazo',
                },
                {
                    text: 'Devolvido em:',
                    align: 'center',
                    sortable: false,
                    value: 'devolucao',
                },
                {
                    text: 'Data do Empréstimo',
                    align: 'center',
                    sortable: false,
                    value: 'created_at',
                },
                {
                    text: 'Livros',
                    align: 'center',
                    sortable: false,
                    value: 'livros',
                },
                { text: "", value: "actions" }
            ],
            emprestimos: [],
            livros: []
        }
    },

    created () {
        this.getEmprestimos();
        this.getLivros();
    },

    methods: {
        async getEmprestimos () {
            this.emprestimos = await this.$axios.$get('http://localhost:3333/emprestimos');
        },
        async getLivros () {
            this.livros = await this.$axios.$get('http://localhost:3333/livros');
        },

        async deleteItem (emprestimo) {
            try {
            if (confirm(`Deseja deletar o empréstimo ID:${emprestimo.id}?`)) {
                let response = await this.$axios.$post('http://localhost:3333/emprestimos/deletar', { id: emprestimo.id });
                this.$toast.success(`Empréstimo ID:${emprestimo.id} deletado com sucesso!`)
                this.getEmprestimos();
            } 
            } catch (error) {
            this.$toast.error('Ocorreu um erro ao atender a requisição. Contate o Gabriel.')
        }
     },

      async endEmprestimo (emprestimo) {
        try {
          if (confirm(`Deseja encerrar o empréstimo ID:${emprestimo.id}?`)) {
            let response = await this.$axios.$post(`http://localhost:3333/emprestimos/${emprestimo.id}`, { devolucao: (new Date(Date.now())).toISOString().substr(0,10) });
                this.$toast.success(`Empréstimo ID:${emprestimo.id} encerrado com sucesso!`)
                this.getEmprestimos();
          }
        } catch (error) {
          this.$toast.error('Ocorreu um erro ao atender a requisição. Contate o Gabriel.')
        }
      }
  }
}
</script>