<template>
  <v-container>
    <v-container style="margin-left:35%">
        <h1>Carcule do Murigol</h1>
        </v-container>
  <v-container style="margin-left:40%; width:25%">
    <v-row>
        <v-text-field
        v-model="visor"
        placeholder="0"
        disabled
        outlined
        style="border:double"
        >
        </v-text-field>
    </v-row>
        </v-container>
  <v-container style="margin-left:40%">
    <v-row >
      <v-col>
        <v-btn
         @click="Entrada('7')"
        >
        7
        </v-btn>
        <v-btn
         @click="Entrada('8')"
        >
        8
        </v-btn>
        <v-btn
         @click="Entrada('9')"
        >
        9
        </v-btn>
        <v-btn 
         @click="Entrada('+')"
        >
        +
        </v-btn>
      </v-col>
    </v-row>
    <v-row>
        <v-col>
            <v-btn
         @click="Entrada('4')"
        >
        4
        </v-btn>
        <v-btn
         @click="Entrada('5')"
        >
        5
        </v-btn>
        <v-btn
         @click="Entrada('6')"
        >
        6
        </v-btn>
        <v-btn
         @click="Entrada('-')"
        >
        -
        </v-btn>
        </v-col>
    </v-row>
    <v-row>
        <v-col>
            <v-btn
         @click="Entrada('1')"
        >
        1
        </v-btn>
        <v-btn
         @click="Entrada('2')"
        >
        2
        </v-btn>
        <v-btn
         @click="Entrada('3')"
        >
        3
        </v-btn>
        <v-btn
         @click="Entrada('/')"
        >
        /
        </v-btn>
        </v-col>
    </v-row>
    <v-row>
        <v-col>
            <v-btn
         @click="Entrada('0')"
        >
        0
        </v-btn>
        <v-btn
         @click="Entrada('=')"
        >
        =
        </v-btn>
        <v-btn
         @click="Entrada('*')"
        >
        *
        </v-btn>
            <v-btn
            @click="Limpar"
            >
                LIMPAR
            </v-btn>
        </v-col>
        </v-row>
        </v-container>
        <v-container>
    </v-container>
  </v-container>
</template>

<script>

export default {
    name: 'CarculePage',

    data () {
        return {
            visor: '',
            numero: [],
            numeros: ['1', '2', '3', '4', '5', '6', '7', '8', '9', '0'],
            operacoes: ['+', '-', '=', '/', '*'],
            operacao: null,
            numero1: null,
            numero2: null
        }
    },

    methods: {
        Entrada(value) {
            if (this.numeros.includes(value)) {
                this.numero.push(value);
                this.visor += this.numero.join('')
            } else {
                this.operacao = value
                this.visor += this.operacao
                if (!this.numero1) {
                    this.numero1 = Number(this.numero.join(''))
                } else {
                    this.operacao = value;
                    this.numero2 = Number(this.numero.join(''));
                }
                if (this.operacao === '='){
                    this.Conta (this.numero1, this.operacao, this.numero2)
                    this.visor = ''
                }
            }
        },

                Limpar() {
                    this.visor = [null]
                },

                Conta (numero1, operacao, numero2) {
                    switch (operacao) {
                        case '+':
                            this.numero1 = Number(numero1) + Number(numero2)
                            break;
                    
                        case '-':
                            this.numero1 = numero1 - numero2
                            this.visor = this.numero1
                            break;
                        
                        case '*':
                            this.numero1 = numero1 * numero2
                            this.visor = this.numero1
                            break;

                        case '/':
                            this.numero1 = numero1 / numero2
                            this.visor = this.numero1
                            break;
                    }
                }

    }

}
</script>