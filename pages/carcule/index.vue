<template>
  <v-container style="background-color: blue;">
    <v-container style="background-color: red;">
      <v-form style="padding-right: 30%; padding-left: 30%;">
        <v-row>
          <v-col cols="12">
            <v-text-field
              v-model="tela"
              placeholder="Carculo"
              full-width
              solo
              disabled
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="3">
            <v-btn
              block
              @click="pegarNumeros(7)"
              solo
            >
              7
            </v-btn>
          </v-col>
          <v-col cols="3">
            <v-btn
              block
              @click="pegarNumeros(8)"
              solo
            >
              8
            </v-btn>
          </v-col>
          <v-col cols="3">
            <v-btn
              block
              @click="pegarNumeros(9)"
              solo
            >
              9
            </v-btn>
          </v-col>
          <v-col cols="3">
            <v-btn
              block
              @click="pegarSinal('+')"
              solo
            >
              +
            </v-btn>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="3">
            <v-btn
              block
              @click="pegarNumeros(4)"
              solo
            >
              4
            </v-btn>
          </v-col>
          <v-col cols="3">
            <v-btn
              block
              @click="pegarNumeros(5)"
              solo
            >
              5
            </v-btn>
          </v-col>
          <v-col cols="3">
            <v-btn
              block
              @click="pegarNumeros(6)"
              solo
            >
              6
            </v-btn>
          </v-col>
          <v-col cols="3">
            <v-btn
              block
              @click="pegarSinal('-')"
              solo
            >
              -
            </v-btn>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="3">
            <v-btn
              block
              @click="pegarNumeros(1)"
              solo
            >
              1
            </v-btn>
          </v-col>
          <v-col cols="3">
            <v-btn
              block
              @click="pegarNumeros(2)"
              solo
            >
              2
            </v-btn>
          </v-col>
          <v-col cols="3">
            <v-btn
              block
              @click="pegarNumeros(3)"
              solo
            >
              3
            </v-btn>
          </v-col>
          <v-col cols="3">
            <v-btn
              block
              @click="pegarSinal('/')"
              solo
            >
              /
            </v-btn>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="6">
            <v-btn
              block
              @click="pegarNumeros(0)"
              solo
            >
              0
            </v-btn>
          </v-col>
          <v-col cols="3">
            <v-btn
              block 
              @click="pegarSinal('=')"
              solo
            >
              =
            </v-btn>
          </v-col>
          <v-col cols="3">
            <v-btn
              block
              @click="pegarSinal('*')"
              solo
            >
              *
            </v-btn>
          </v-col>
        </v-row>
      </v-form>
    </v-container>
  </v-container>
</template>

<script>

export default {
  name: 'CarculadoraPage',

  data() {
    return {
      aux: [],
      tela: null,
      numero: [],
      numerosCompostos: [],
    }
  },

  methods:{
    pegarNumeros(numeros) {
      this.numero.push(numeros);
      this.tela = this.push(this.numero);
      this.numero = this.numero.join('');
      this.numerosCompostos.push(this.numero);
    },

    pegarSinal(sinal) {
      this.tela = this.sinal
      this.numerosCompostos.push(sinal);
    },

    quebra(sinal) {
      if(tela = '+') {
        this.numerosCompostos = this.numeroAtual;
        this.numeroAtual = [];
        this.numerosCompostos.push(sinal)
        console.log(`${this.numerosCompostos}`)
      }
    }
  }
}
</script>