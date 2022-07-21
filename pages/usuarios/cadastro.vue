<template>
  <v-container>
    <h1>Cadastro de Usuários</h1>
    <hr>
    <v-form>
    <v-container>
        <v-row>
            <v-col
            cols="2"
            >
                <v-text-field
                    v-model="usuario.id"
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
                    v-model="usuario.nome"
                    placeholder="Nome"
                    label="Nome"
                    outlined
                >
                </v-text-field>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <v-text-field
                    v-model="usuario.cpfcnpj"
                    placeholder="CPF/CNPJ"
                    label="CPF/CNPJ"
                    outlined
                >
                </v-text-field>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <v-text-field
                    v-model="usuario.email"
                    placeholder="E-mail"
                    label="E-mail"
                    outlined
                >
                </v-text-field>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <v-text-field
                    v-model="usuario.telefone"
                    placeholder="Telefone"
                    label="Telefone"
                    outlined
                >
                </v-text-field>
            </v-col>
        </v-row>
    </v-container>
    </v-form>
    <v-btn
        outlined
        to="/usuarios"
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
    name: 'CadastroUsuariosPage',

    data () {
        return {
            usuario: {
                id: null,
                nome: null,
                cpfcnpj: null,
                email: null,
                telefone: null
            }
        }
    },

    methods: {
        async cadastrar () {
            try {
                let usuario = {
                nome: this.usuario.nome,
                cpfcnpj: this.usuario.cpfcnpj,
                email: this.usuario.email,
                telefone: this.usuario.telefone
            }
                let response = await this.$axios.$post('http://localhost:3333/usuarios', usuario);
                this.$router.push('/usuarios')
                this.$toast.success(`Usuário ID ${response.id} cadastrado com sucesso!`);
            } catch (error) {
                this.$toast.error('Impossível concluir a operação, contate o suporte.')
            }
    }
}
}
</script>