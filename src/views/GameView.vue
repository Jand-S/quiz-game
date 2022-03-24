<template>
  <div class="container">
    <div v-if="errou === false">
      <div v-for="(pergunta, n) in perguntas" :key="pergunta.index">
        <div v-if="perguntaAtual === n" class="pergunta">
          <div class="titulo">{{pergunta.pergunta}}</div>
          <div class="respostas">
            <button v-for="(res, resposta) in pergunta.respostas" :key="res.index"
                    @click="responder(resposta, n)" class="btn-resposta">
              {{res}}
              {{index}}
            </button>
          </div>
        </div>
      </div>
    </div>
    <div v-else>
      <div class="errou">
        <div>
          <audio>
            <track src="src/assets/jumpscare.mp3" kind="captions">
          </audio>
          <img alt="jump-scare" src="src/assets/jump.jpg">
        </div>
        <button class="btn-voltar" @click="voltar()">voltar</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'GameView',
  data() {
    return {
      perguntas: null,
      perguntaAtual: 0,
      errou: false,
    };
  },
  mounted() {
    axios.get('https://my-json-server.typicode.com/Jand-S/quiz-game/quiz')
      .then((res) => {
        this.perguntas = res.data;
      });
  },
  methods: {
    responder(resposta, pergunta) {
      if (this.perguntas[pergunta].certa === resposta + 1) {
        console.log('acertou');
        this.perguntaAtual += 1;
      } else {
        this.errou = true;
      }
    },
    voltar() {
      this.perguntaAtual = 0;
      this.errou = false;
    },
  },
};

</script>

<style scoped>

.container {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 70vh;
}

.titulo {
  font-size: 60px;
  color: white;
}

.respostas {
  display: flex;
  width: 440px;
  flex-wrap: wrap;
  margin: auto;
  margin-top: 15px;
}

.btn-voltar {
  width: 100px;
  height: 30px;
  margin: 10px;
  border-radius: 5px;
  background-color: darkred;
  color: white;
  border: 2px solid #111;
}

.btn-voltar:hover {
  filter: brightness(0.8);
  cursor: pointer;
}

.btn-resposta {
  width: 200px;
  height: 70px;
  margin: 10px;
  border-radius: 10px;
  background-color: darkslategrey;
  color: white;
  border: 2px solid #111;
}

.errou {
  color: darkred;
  font-size: 50px;
  font-weight: bold;
}

.btn-resposta:hover {
  filter: brightness(0.8);
  cursor: pointer;
}

</style>
