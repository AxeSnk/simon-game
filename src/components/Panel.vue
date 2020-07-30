<template>
  <div class="panel-wrapper">
    <div
      v-for="item in itemPanel"
      :key="item.id"
      :class="item.name + ' ' + (item.active ? 'active' : '')"
      @click.prevent="play(item.src)"
      class="panel"
    ></div>
    <div class="display">
      <div class="round">{{round}}</div>
      <button class="btn-start" @click="start">Start</button>
    </div>
  </div>
</template>

<script>
import { items } from "./items";

export default {
  name: "Panel",
  components: {},
  data: () => ({
    itemPanel: items,
    sequence: [],
    copy: [],
    round: 0,
    active: false,
    difficulty: "normal",
  }),
  methods: {
    start() {
      this.sequence = [];
      this.copy = [];
      this.round = 0;
      this.active = true;
      this.newRound();
    },
    newRound() {
      ++this.round;
      this.sequence.push(this.randomNumber());
      this.copy = this.sequence.slice(0);
      this.animate(this.sequence);
    },
    randomNumber() {
      return Math.floor(Math.random() * 4 + 1);
    },
    registerClick(e) {
      const desiredResponse = this.copy.shift();
      this.checkLose();
    },
    checkLose() {
      if (this.copy.length === 0 && this.active) {
        this.deactivateSimonBoard();
        this.newRound();
      } else if (!this.active) {
        this.deactivateSimonBoard();
        this.endGame();
      }
    },
    animate(sequence) {
      let i = 0;
      let audio = require(`../assets/${sequence[i]}.mp3`);
      const that = this;
      const interval = setInterval(() => {
        that.play(audio);
        that.lightUp(sequence[i]);

        i++;
        if (i >= sequence.length) {
          clearInterval(interval);
          that.activateSimonBoard();
        }
      }, 600);
    },
    endGame() {},
    activateSimonBoard(e) {},
    play(audio) {
      const music = new Audio(audio);
      music.play();
    },
    lightUp(id) {
      this.itemPanel[id-1].active = true
      let light = setTimeout(() => {
        this.itemPanel[id-1].active = false
      }, 300)
    }
  },
};
</script>

<style lang="scss">
.panel-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 300px;
  height: 300px;
  flex-wrap: wrap;
  border-radius: 100%;
  position: relative;
}
.panel {
  display: inline-block;
  width: 150px;
  height: 150px;
  cursor: pointer;
  outline: none;
  -webkit-tap-highlight-color: rgba(255, 255, 255, 0);
  -webkit-tap-highlight-color: transparent;
}
.panel:hover {
  box-shadow: inset 0 0 20px 5px cyan;
}
.bottom-panel-right {
  border-bottom-right-radius: 100%;
  background-color: blue;
  opacity: 0.6;
}
.bottom-panel-left {
  border-bottom-left-radius: 100%;
  background-color: yellow;
  opacity: 0.6;
}
.top-panel-right {
  border-top-right-radius: 100%;
  background-color: red;
  opacity: 0.6;
}
.top-panel-left {
  border-top-left-radius: 100%;
  background-color: green;
  opacity: 0.6;
}
.panel:active {
  opacity: 1;
}
.panel.active {
  opacity: 1;
  box-shadow: inset 0 0 20px 5px cyan;
}
.top-panel,
.bottom-panel {
  display: flex;
}
.red {
  background-color: red;
}
.green {
  background-color: green;
}
.blue {
  background-color: blue;
}
.yellow {
  background-color: yellow;
}
.display {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100px;
  height: 100px;
  border-radius: 50%;
  background: white;
  position: absolute;
}
</style>