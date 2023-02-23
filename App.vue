<template>
  <div class="whackamole">
    <h1 class="logo">Whack-a-mole!</h1>
    <button class="start-game" @click="startGame"  >Start Game</button>
    <div class="counters-container">
      <Counter label="Score" v-bind:value="score" />
      <Counter label="High Score" v-bind:value="highScore" />
      <Counter label="Timer" v-bind:value="timer" />
    </div>
    <Moles v-bind:mole1="moles[0]" v-bind:mole2="moles[1]" v-bind:mole3="moles[2]" v-bind:mole4="moles[3]" v-bind:gameActive="activateStart" v-on:event="countUp"/>
  </div>
</template>

<script>
import Counter from './components/Counter.vue';
import Moles from './components/Moles.vue';

export default {
  name: 'App',
  components: {
    Counter,
    Moles,
  },
  data() {
    return {
      timer: 20,
      moles: {mole1: false, mole2: false, mole3: false, mole4: false},
      timerID: null,
      activateStart: false,
      score: 0,
      highScore: 0,
      timerID_mole: null,
    };
  },

  methods: {
    resetState() {
      Object.assign(this.$data, this.$options.data.call(this));
    },
    startGame() {
      if (this.timerID != null) return;
      this.activateStart = !this.activateStart;
      this.startTimer();
      this.activateStart = true;
      this.randMole();
    },
    endGame() {
      this.stopTimer();
      this.stop_randMole();
      var tmp_highScore = this.highScore;
      if(tmp_highScore < this.score){
        tmp_highScore = this.score;
      }
      this.resetState();
      this.highScore = tmp_highScore;
    },
    countUp(moleID) {
      this.score++;
      this.moles[moleID] = false;
    },
    getRandomInt(max) {
      return Math.floor(Math.random() * max);
    },
    randMole() {
      this.timerID_mole = setInterval(() => {
        var randNum = this.getRandomInt(4);
        while(this.moles[randNum]) {
          randNum = this.getRandomInt(4);
        }
        this.moles[randNum] = true;
        setTimeout(() => {
          this.moles[randNum] = false;
        }, 1500);
      }, 500);
    },
    stop_randMole() {
      clearInterval(this.timerID_mole);
    },

    startTimer() {
      this.timerID = setInterval(() => {
        this.timer--;
        if (this.timer == 0){
          this.endGame();
        }
      }, 1000);
    },

    stopTimer() {
      clearInterval(this.timerID);
    }
  },
};
</script>

<style scoped>
.whackamole {
  font-family: 'Bungee', sans-serif;
  max-width: 960px;
  margin: auto;
  text-align: center;
}

.start-game {
  font-family: 'Bungee', sans-serif;
  padding: 20px;
  border-radius: 3px;
  border: 0;
  background-color: #52b1d6;
  color: #fff;
  font-size: 1em;
  cursor: pointer;
}

.counters-container {
  display: flex;
  justify-content: space-evenly;
}
</style>
