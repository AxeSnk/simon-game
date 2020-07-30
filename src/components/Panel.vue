<template>
  <div class="panel-wrapper" :class="hover ? 'hoverable' : '' ">
    <div
      v-for="item in itemPanel"
      :id="item.id"
      :key="item.id"
      :class="item.name + ' ' + (item.active ? 'active' : '')"
      @click="registerClick"
      @mousedown="play(item.src)"
      class="panel"
    ></div>
    <div class="display">
      <div class="round">{{round}}</div>
      <button class="btn-start" @click="start">Start</button>
      <div class="radio-wrapper" @click="changeMode">
        <label for="level">
          Ease
          <input type="radio" name="level" value="1500" checked />
        </label>
        <label for="level">
          Medium
          <input type="radio" name="level" value="1000" />
        </label>
        <label for="level">
          Hard
          <input type="radio" name="level" value="400" />
        </label>
      </div>
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
    active: true,
    hover: false,
    difficulty: "normal",
    mode: 1500,
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
      const actualResponse = e.target.id;
      this.active = desiredResponse == actualResponse;
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
      const that = this;
      const interval = setInterval(() => {
        let audio = require(`../assets/${sequence[i]}.mp3`);
        that.play(audio);
        that.lightUp(sequence[i]);

        i++;
        if (i >= sequence.length) {
          clearInterval(interval);
          that.activateSimonBoard();
        }
      }, this.mode);
    },
    endGame() {
      alert("Game over!");
    },
    activateSimonBoard() {
      this.hover = true;
    },
    deactivateSimonBoard() {
      this.hover = false;
    },
    play(audio) {
      const music = new Audio(audio);
      music.play();
    },
    lightUp(id) {
      this.itemPanel[id - 1].active = true;
      let light = setTimeout(() => {
        this.itemPanel[id - 1].active = false;
      }, 300);
    },
    changeMode(e) {
      this.mode = Number(e.target.value);
    },
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
  pointer-events: none;
  outline: none;
  -webkit-tap-highlight-color: rgba(255, 255, 255, 0);
  -webkit-tap-highlight-color: transparent;
}
.hoverable .panel {
  pointer-events: auto;
}
.hoverable .panel:hover {
  box-shadow: inset 0 0 20px 5px cyan;
	cursor: pointer;
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
  width: 120px;
  height: 120px;
  border-radius: 50%;
  background: white;
  position: absolute;
}
.round {
  font-size: 1.3rem;
  font-weight: bold;
}
.btn-start {
  border: 1px solid black;
  background-color: lightcyan;
  border-radius: 10px;
  padding: 2px 18px;
  font-size: .9rem;
  font-weight: bold;
  font-family:'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
  letter-spacing: 1.2px;
  cursor: pointer;
  box-shadow: 2px 2px 15px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease 0s;
}
.btn-start:active {
  transform: translateY(3px);
}
.btn-start,
.btn-start:hover,
.btn-start:active {
  outline: none;
  -webkit-tap-highlight-color: rgba(255, 255, 255, 0);
  -webkit-tap-highlight-color: transparent;
}
.radio-wrapper {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-end;

  label, input {
    cursor: pointer;
  }
}
</style>