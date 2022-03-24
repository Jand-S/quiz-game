<template>
  <div class="container">
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
</template>

<script>
import axios from 'axios';

export default {
  name: 'GameView',
  data() {
    return {
      perguntas: null,
      perguntaAtual: 0,
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
        this.perguntaAtual = 0;
      }
    },
  },
};

</script>

<style scoped>

.container {
  display: flex;
  align-items: center;
  /*width: 720px;*/
  justify-content: center;
  height: 70vh;
}

.titulo {
  font-size: 60px;
  color: white;
}

.pergunta {}

.respostas {
  display: flex;
  width: 440px;
  flex-wrap: wrap;
  margin: auto;
  margin-top: 15px;
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

.btn-resposta:hover {
  filter: brightness(0.8);
  cursor: pointer;
}

</style>
