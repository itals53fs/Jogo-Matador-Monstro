<template>
 <div id="app">
            <div id="wrapper">
                <h1>Jogo Matador de Monstro</h1>
            </div>
            <div class="card">

                <div class="jogadores">Jogador
                    <div class="progress">
                        <div class="centro" :style="[{width:jogador.vida+'%'}]" :class="redJ"></div>
                        {{jogador.vida}}%
                    </div>
                </div>

                <div class="jogadores">Monstro
                    <div class="progress">
                        <div class="centro" :style="{width:monstro.vida+'%'}" :class="redM"></div>
                        {{monstro.vida}}%
                    </div>
                </div>


            </div>
            <div class="res card" v-show="res">
                <div v-if="result" :style="{color:'green'}">Voce ganhou :)</div>
                <div v-else :style="{color:'red'}">Voce perdeu :(</div>
            </div>

            <div class="card">
                <button v-if="!init" @click="iniciar" style="background-color: royalblue;">INICIAR NOVO JOGO</button>
                <template v-else>
                    <button @click="ataque(5,15)" style="background-color: orangered;">ATAQUE</button>
                    <button @click="ataque(5,15)" style="background-color: darkgoldenrod;">ATAQUE ESPECIAL</button>
                    <button @click="curar" style="background-color: darkgreen;">CURAR</button>
                    <button @click="desistir" style="background-color:grey;">DESISTIR</button>
                </template>


            </div>
            <div class="card">
                <div>
                    <ul class="ul">
                        <li v-for="ataque in jogador.ataque" class="res-jogador" :key="ataque">{{ataque}}</li>
                    </ul>
                </div>
                <div>
                    <ul class="ul">
                        <li v-for="ataque in monstro.ataque" class="res-monstro" :key="ataque">{{ataque}}</li>
                    </ul>
                </div>
            </div>
        </div>
</template>

<script>
export default {
  //el: "#app",
  name: "HelloWorld.vue",
 data() {
   return{
            init: false,
            result: false,
            redJ: false,
            redM: false,
            res: false,
            jogador: {
                vida: 100,
                ataque: []
            },
            monstro: {
                vida: 100,
                ataque: []
            },
        }
 },
        methods: {
            iniciar() {
                this.init = !this.init;
                this.jogador.vida = 100;
                this.jogador.ataque = [];
                this.monstro.vida = 100;
                this.monstro.ataque = [];
                this.res = false;
                this.redJ = false;
                this.redM = false;
            },
            ataque(j, m) {
                const ataqueJ = (parseInt(Math.random() * 10) - j) + j;
                const ataqueM = (parseInt(Math.random() * 10) - m) + m;

                this.jogador.vida = this.jogador.vida - ataqueM;
                this.monstro.vida = this.monstro.vida - ataqueJ;

                this.jogador.ataque.unshift(`JOGADOR ATINGIU MONSTRO COM ${ataqueJ}`);
                this.monstro.ataque.unshift(`MONSTRO ATINGIU JOGADOR COM ${ataqueM}`);

                if (this.jogador.vida <= 0) {
                    this.jogador.vida = 0;
                    this.res = true;
                    this.result = false;
                    this.desistir();
                }


                if (this.monstro.vida <= 0) {
                    this.monstro.vida = 0;
                    this.res = true;
                    this.result = true;
                    this.desistir();
                }

                this.colorProgess();

            },
            curar() {
                const forca = parseInt(Math.random() * 10);
                const ataque = parseInt(Math.random() * 10);

                this.jogador.vida -= ataque;
                this.jogador.vida += forca;
                this.jogador.ataque.unshift(`JOGADOR RECEBEU FORÇA DE ${forca}`);
                this.monstro.ataque.unshift(`MONSTRO ATINGIU JOGADOR COM ${ataque}`);
                if (this.jogador.vida + forca >= 100) {
                    this.jogador.vida = 100;
                }
                this.colorProgess();

            },
            desistir() {
                this.init = false;
            },
            colorProgess() {
                if (this.jogador.vida < 21) {
                    this.redJ = "red";
                } else {
                    this.redJ = false;
                }
                if (this.monstro.vida < 21) {
                    this.redM = "red";
                } else {
                    this.redM = false;

                }
            }

        }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    * {
        font-family: monospace;
        font-size: 100%;
    }

    .card {
        display: flex;
        justify-content: center;
        box-shadow: 1px 1px 1px 1px teal;
        margin: 20px;
        padding: 10px;
        width: 90%;
    }

    .jogadores {
        text-align: center;
        line-height: 50px;
        padding: 30px;
        width: 100%;

    }

    .jogadores div {
        text-align: center;
        height: 30px;
        line-height: 30px;
        width: 100%;
    }

    .progress {
        border: solid 1px black;
        width: 100%;
        height: 100%;

    }

    .centro {

        background-color: chartreuse;
    }

    .res {
        font-weight: bold;
    }

    .ul li {
        font-size: 14px;
        list-style: none;
        margin-top: 5px;
        padding: 5px;
    }

    .res-jogador {
        background-color: darkcyan;
    }

    .res-monstro {
        background-color: chocolate;
    }
    .red{
        background-color: crimson;
    }

    button {
        width: 45%;
        text-align: center;
        margin: 5px;
    }
</style>
