<template>
  <v-container>
    <h1>Cadastro de Autores</h1>
    <hr>
    <v-form v-model="valid">
    <v-container>
        <v-row>
            <v-col
            cols="2"
            >
                <v-text-field
                    v-model="autor.id"
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
                    v-model="autor.nome"
                    placeholder="Nome"
                    label="Nome"
                    :rules="rule"
                    required
                    outlined
                >
                </v-text-field>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <v-text-field
                    v-model="autor.email"
                    placeholder="E-mail"
                    label="E-mail"
                    :rules="rule"
                    required
                    outlined
                >
                </v-text-field>
            </v-col>
        </v-row>
    </v-container>
    </v-form>
    <v-btn
        outlined
        to="/autores"
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
    name: 'CadastroAutoresPage',

    data () {
        return {
            valid: false,
            autor: {
                id: null,
                nome: null,
                email: null
            },
            rule: [
                v => !!v || 'Esse campo é obrigatório'
            ]
        }
    },

    created () {
    if (this.$route?.params?.id) {
      this.getById(this.$route.params.id)
    }
  },

    methods: {
        async persistir () {
            try {
                //primeiro valida-se se o formulário está preenchido
                if (!this.valid) {
                    return this.$toast.warning('O formulário de cadastro não é válido!')
                }
                //montamos a variárel autor para enviar nos posts
                let autor = {
                    nome: this.autor.nome,
                    email: this.autor.email
                }
        //caso não tenha ID na tela, significa que é um cadastro NOVO
        //por isso ele vai apenas com o objeto da autor para o cadastro
        //como no final tem um RETURN, ele vai cair fora da função PERSISTIR
        if (!this.autor.id) {
          await this.$axios.$post('http://localhost:3333/autores', autor);
          this.$toast.success('Cadastro realizado com sucesso!');
          return this.$router.push('/autores');
        }
        await this.$axios.$post(`http://localhost:3333/autores/${this.autor.id}`, autor);
        this.$toast.success('Cadastro atualizado com sucesso!');
        return this.$router.push('/autores');
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
      }
    },
    async getById (id) {
      this.autor = await this.$axios.$get(`http://localhost:3333/autores/${id}`);
    }
  }
}
</script>