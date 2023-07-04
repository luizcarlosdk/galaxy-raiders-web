<template>
  <div id="leaderboard">

    <div class="players">
      <p class="textDecorated">Primeiro lugar</p>
      <p class="subText">Pontuação: {{ scores[0].score }}</p>
      <p class="subText">Asteroides destruidos: {{ scores[0].asteroidsDestroyed }}</p>
    </div>
    <div class="players">
      <p class="textDecorated">Segundo lugar</p>
      <p class="subText">Pontuação: {{ scores[1].score }}</p>
      <p class="subText">Asteroides destruidos: {{ scores[1].asteroidsDestroyed }}</p>
    </div>
    <div class="players">
      <p class="textDecorated">Terceiro lugar</p>
      <p class="subText">Pontuação: {{ scores[2].score }}</p>
      <p class="subText">Asteroides destruidos: {{ scores[2].asteroidsDestroyed }}</p>
    </div>

  </div>
</template>

<script>

import leaderboard from "../assets/leaderboard.json"

export default {

  teste() {
  },

  data: () => ({
    scores: leaderboard

  }),

}



</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Anton&family=Dancing+Script:wght@400;700&family=Lobster&display=swap');

.textDecorated {
  font-family: 'Anton', sans-serif;
  font-size: 30px;
  color:pink

}

.subText {
  font-family: 'Lobster', cursive;
  font-size: 15px;
  color: cyan
}

#leaderboard {
  width: 40%;
  display: flex;
  align-items: center;
  flex-direction: column;
}

.players {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 40%;
  height: 40%;
  opacity: 80%;
}</style>