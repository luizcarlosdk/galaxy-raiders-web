<template>
  <div id="canvas">
    <div id="deep-space" />
    <div id="space-field">
      <Leaderboard v-if="showLearderboard" />
      <div id="menu" v-show="menuOpen">
        <img src="~/assets/play.png" id="play" v-on:click="playGame" v-if="!gameStarted">
        <img src="~/assets/leaderboard.png" v-on:click="showLeader" width="80%">
        <img src="~/assets/quit.png" width="45%" v-on:click="quitGame">
      </div>
      <SpaceObject id="spaceship" class="spaceship" :data="spaceField.ship" resolution="2" v-if="gameStarted" />

      <SpaceObject class="asteroid" :data="asteroid" resolution="2" :key="asteroid.center"
        v-for="asteroid in spaceField.asteroids" />

      <SpaceObject class="missile" :data="missile" resolution="2" :key="missile.center"
        v-for="missile in spaceField.missiles" />

      <SpaceObject class="explosion" :data="explosion" resolution="2" :key="explosion.center"
        v-for="explosion in spaceField.explosions" />
    </div>
  </div>
</template>

<script setup>

import Leaderboard from '~~/components/Leaderboard.vue';

let menuOpen = 1;
let gameStarted = 0
let showLearderboard = false

const playGame = () => {
  menuOpen = 0;
  gameStarted = 1;
}

const quitGame = () => {
  $post("/quit")
};

const showLeader = () => {
  showLearderboard = showLearderboard === 1 ? 0 : 1;
}

const {
  data: spaceField,
  refresh: updateSpaceField
} = await $get("/space-field");

onMounted(() => {
  window.addEventListener("keydown", async (event) => {
    const keyToCommand = {
      "ArrowUp": "MOVE_SHIP_UP",
      "ArrowDown": "MOVE_SHIP_DOWN",
      "ArrowRight": "MOVE_SHIP_RIGHT",
      "ArrowLeft": "MOVE_SHIP_LEFT",
      "Space": "LAUNCH_MISSILE",
      "Escape": "PAUSE_GAME",
    };
    const command = keyToCommand[event.code];

    // Ignore if invalid key was pressed
    if (command === undefined) return;

      if (command === 'PAUSE_GAME') {
          menuOpen = menuOpen === 1 ? 0 : 1;
          
      }
      console.log(`Triggering command: ${command}`);
      await $post("/ship/commands", { command })

  });

  window.setInterval(updateSpaceField, 16);
})
</script>

<style>
#play {
  width: 50%;
}

#menu {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 40%;
  height: 40%;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  opacity: 80%;
}


#canvas {
  height: calc(100vh - 4rem);
  width: calc(100vw - 4rem);

  padding: 2rem;

  background-color: #36bbf5;
  overflow: hidden;

  position: relative;

  display: flex;
  justify-content: center;
  align-items: center;
}

@keyframes slide {
  0% {
    transform: translate(1px);
  }

  50% {
    transform: translate(-1px);
  }

  100% {
    transform: translate(1px);
  }
}

#deep-space {
  height: calc(100% - 4rem);
  width: calc(100% - 4rem);

  background-image: url("~/assets/space.png");
  background-origin: content-box;
  animation: slide 3s linear infinite;

  position: absolute;
  z-index: 0;
}

#space-field {
  height: calc(100% - 4rem);
  width: calc(100% - 4rem);

  position: relative;
}

.spaceship {
  background-image: url("~/assets/spaceship.png");
}

.asteroid {
  background-image: url("~/assets/asteroid.png");
}

.missile {
  background-image: url("~/assets/missile.png");
}

.explosion {
  background-image: url("~/assets/explosion.gif");
}
</style>
