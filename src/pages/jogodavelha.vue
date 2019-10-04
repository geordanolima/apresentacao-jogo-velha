<template>
    <div class="row col">
        <q-page class="col-3 justify-center">
            <q-input
                v-model="jogador1"
                class="jogador col paddin-top-100"
                color="white"
                align="right"
                dark
                clearable
                @keydown.enter="$_cadastraJogador1"
            />
            <p class="pSimbolo">X</p>
        </q-page>
        <q-page class="container justify-center items-center col">
            <div>
                <div class="flex flex-center col-md-10">
                    <q-btn
                        class="reset"
                        @click="reiniciar"
                        icon="refresh"
                        color="accent"
                        round
                    >
                        <q-tooltip>
                            <p style="font-size:15pt">
                                Reiniciar a partida
                            </p>
                        </q-tooltip>
                    </q-btn>
                </div>
                <div class="flex flex-center col-md-10 jogador">
                    <span> JOGADOR {{ vezDe == 'X' ? jogador1 : jogador2 }}
                        <h1 v-show="alguemGanhou">GANHOU</h1>
                    </span>
                </div>

            </div>
            <q-page class="flex flex-center col-md-10">
                <div>
                    <jv-board
                        v-model="posicoes"
                        @jogar="jogarPartida"
                    />
                </div>
            </q-page>
        </q-page>
        <q-page class="col-3">
            <q-input
                v-model="jogador2"
                class="jogador col paddin-top-100"
                color="white"
                align="right"
                dark
                clearable
                @keydown.enter="$_cadastraJogador2"
            />
        </q-page>
    </div>
</template>

<script>
import jvBoard from '../components/jvBoard'
import { Notify } from 'quasar'
import axios from 'axios'
export default {
  name: 'jogovelha',

  components: { jvBoard },

  data () {
    return {
      vezDe: 'X',
      alguemGanhou: false,
      posicoes: {
        A1: {
          exibir: null,
          ganhou: false
        },
        A2: {
          exibir: null,
          ganhou: false
        },
        A3: {
          exibir: null,
          ganhou: false
        },
        B1: {
          exibir: null,
          ganhou: false
        },
        B2: {
          exibir: null,
          ganhou: false
        },
        B3: {
          exibir: null,
          ganhou: false
        },
        C1: {
          exibir: null,
          ganhou: false
        },
        C2: {
          exibir: null,
          ganhou: false
        },
        C3: {
          exibir: null,
          ganhou: false
        }
      },
      jogador1: 'jogador1',
      jogador2: 'jogador2',
      id_jogador1: null,
      id_jogador2: null,
      url: 'http://127.0.0.1:8000'
    }
  },
  computed: {
    jogada () {
      const getExibir = i => i.exibir
      const getValues = value => Object.values(value).map(getExibir)
      return getValues(this.posicoes)
    }
  },
  methods: {
    $_cadastraJogador1 () {
      this.id_jogador1 = this.$_cadastrarJogador(this.jogador1)
    },
    $_cadastraJogador2 () {
      this.id_jogador2 = this.$_cadastrarJogador(this.jogador2)
    },
    $_cadastrarJogador (value) {
      axios.post(
        '/usuarios/', {
          'nome': value
        }, {
          baseURL: this.url
        })
        .then(response => {
          return response.data.id
        })
    },
    $_adicionaVencedorPartida (idJogador) {
      axios.post(
        `/paartidas/`, {
          'id_usuario': idJogador,
          'vencedor': true
        }, {
          baseURL: this.url
        })
        .then(response => {
          console.log(response.data)
        })
    },
    jogarPartida (jogada) {
      let vezDeJogar = this.vezDe
      let ninguemGanhou = !this.alguemGanhou
      if (ninguemGanhou) {
        jogada.exibir = vezDeJogar
        this.validaGanhador(this.jogada)
        if (ninguemGanhou) {
          this.vezDe = vezDeJogar === 'X' ? 'O' : 'X'
        }
      }
    },
    validaGanhador (lista) {
      let valida = this.vezDe
      if (((lista[0] === valida && lista[1] === valida && lista[2] === valida) || (lista[3] === valida && lista[4] === valida && lista[5] === valida) || (lista[6] === valida && lista[7] === valida && lista[8] === valida)) ||
                ((lista[0] === valida && lista[3] === valida && lista[6] === valida) || (lista[1] === valida && lista[4] === valida && lista[7] === valida) || (lista[2] === valida && lista[5] === valida && lista[8] === valida)) ||
                ((lista[0] === valida && lista[4] === valida && lista[8] === valida) || (lista[2] === valida && lista[4] === valida && lista[6] === valida))) {
        Notify.create({
          message: `Parabéns, ${this.vezDe === 'X' ? this.jogador1 : this.jogador2}, você venceu esta partida!`,
          closeBtn: 'fechar',
          position: 'center',
          type: 'positive',
          textColor: 'black',
          color: 'white',
          timeout: 3000
        })
        this.$_adicionaVencedorPartida(this.vezDe === 'X' ? this.id_jogador1 : this.id_jogador2)
        setTimeout(() => {
          this.reiniciar()
        }, 3000)
      }
    },
    reiniciar () {
      for (const item in this.posicoes) {
        let posicao = this.posicoes[item]
        posicao.exibir = null
        posicao.ganhou = false
      }
      this.vezDe = 'X'
      this.alguemGanhou = false
    }
  }
}
</script>

<style lang="scss">
.reset{
    margin: 3%;
}
.jogador {
    color: #fff !important;
    font-size: 35pt;
    font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
        "Lucida Sans Unicode", Geneva, Verdana, sans-serif;
}
.paddin-top-100{
    padding-top: 250px;
}
.q-field__native{
    color: #fff !important;
    text-align: center;
}
.q-field__marginal{
    color: #fff !important;
}
.flex-center{
    min-height: 0px !important;
}
.pSimbolo{
    color: #fff;
    font-size: 45pt;
    text-align: center;
}
</style>
