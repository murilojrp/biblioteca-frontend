<template>
  <v-container>
    <h1>Cadastro de Empréstimos</h1>
    <hr>
    <v-form v-model="valid">
    <v-container>
        <v-row>
            <v-col
            cols="2"
            >
              <v-text-field
                  v-model="emprestimo.id"
                  placeholder="Código"
                  label="Código"
                  disabled
                  outlined
              />
            </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-dialog
              ref="dialog"
              v-model="modal"
              :return-value.sync="date"
              persistent
              width="290px"
            >
              <template v-slot:activator="{ on, attrs }">
                <v-text-field
                  v-model="emprestimo.prazo"
                  label="Prazo"
                  prepend-icon="mdi-calendar"
                  disabled
                  readonly
                  v-bind="attrs"
                  v-on="on"
                ></v-text-field>
              </template>
              <v-date-picker
                v-model="emprestimo.prazo"
                :disabled="desabilitar"
                scrollable
              >
                <v-spacer></v-spacer>
                <v-btn
                  text
                  color="primary"
                  @click="modal = false"
                >
                  Cancel
                </v-btn>
                <v-btn
                  text
                  color="primary"
                  @click="$refs.dialog.save(date)"
                >
                  OK
                </v-btn>
              </v-date-picker>
            </v-dialog>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="emprestimo.devolucao"
              placeholder="Devolução"
              label="Devolvido em:"
              :disabled="desabilitar"
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-autocomplete
              v-model="emprestimo.idUsuario"
              :items="usuarios"
              :rules="rule"
              required
              :disabled="desabilitar"
              outlined
              label="Usuário"
              item-text="nome"
              item-value="id"
            />
          </v-col>
          <v-col>
            <v-autocomplete
              multiple
              v-model="emprestimo.livros"
              :items="livros"
              :rules="rule"
              required
              :disabled="desabilitar"
              outlined
              label="Livros"
              item-text="titulo"
              item-value="id"
              chips
            />
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-btn
      outlined
      to="/emprestimos"
    >
      Cancelar
    </v-btn>
    <v-btn
      outlined
      @click="persistir"
    >
      Salvar
    </v-btn>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroEmprestimosPage',

  data () {
    return {
      desabilitar: false,
      livros: [], 
      usuarios: [],
      valid: false,
      emprestimo: {
        id: null,
        prazo: null,
        devolucao: null,
        idUsuario: null
      },
      rule: [
        v => !!v || 'Esse campo é obrigatório'
      ]
    }
  },

  created () {
    if (this.$route?.params?.id) {
      this.getById(this.$route.params.id);
    }
    this.getLivros();
    this.getUsuarios();
  },
  
  methods: {
    async persistir () {
      try {
        //primeiro valida-se se o formulário está preenchido
        if (!this.valid) {
          return this.$toast.warning('O formulário de cadastro não é válido!')
        }
        //montamos a variárel categoria para enviar nos posts
        let emprestimo = {
          prazo: this.emprestimo.prazo,
          devolucao: this.emprestimo.devolucao,
          idUsuario: this.emprestimo.idUsuario,
          livros: this.emprestimo.livros
        }
        //caso não tenha ID na tela, significa que é um cadastro NOVO
        //por isso ele vai apenas com o objeto da categoria para o cadastro
        //como no final tem um RETURN, ele vai cair fora da função PERSISTIR
        if (!this.emprestimo.id) {
          await this.$axios.$post('http://localhost:3333/emprestimos', emprestimo);
          this.$toast.success('Cadastro realizado com sucesso!');
          return this.$router.push('/emprestimos');
        }
        await this.$axios.$post(`http://localhost:3333/emprestimos/${this.emprestimo.id}`, emprestimo);
        this.$toast.success('Cadastro atualizado com sucesso!');
        return this.$router.push('/emprestimos');
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
      }
    },
  
    async getById (id) {
      this.emprestimo = await this.$axios.$get(`http://localhost:3333/emprestimos/${id}`);
      this.desabilitar = true;
    },

    async getLivros() {
      this.livros = await this.$axios.$get('http://localhost:3333/livros');
    },

    async getUsuarios() {
      this.usuarios = await this.$axios.$get('http://localhost:3333/usuarios');
    }
  }
}
</script>