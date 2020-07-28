<template>
  <div class="panel-wrapper">
    <div class="top-panel">
      <div v-on:click="activateSimonBoard" class="panel top-left-panel" data-id="1">
        <audio autoplay v-if="sequence">
          <source src="assets/' + tile + '.mp3" type="audio/mp3" />
        </audio>
      </div>
      <div @click="activateSimonBoard" class="panel top-right-panel" data-id="2"></div>
    </div>
    <div class="bottom-panel">    
      <div @click="activateSimonBoard" class="panel bottom-left-panel" data-id="3"></div>
      <div @click="activateSimonBoard" class="panel bottom-right-panel" data-id="4"></div>
    </div>
    <div class="display">
      <button class="btn-start" @click="start">Start</button>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Panel',
  components: {
  },
  data: () => ({
    sequence: [],
		copy: [],
		round: 0,
		active: true,
		mode: 'normal',
  }),
  methods: {
    start() {
      this.sequence = [];
      this.copy = [];
      this.round = 0;
      this.active = true;
      this.newRound()
    },
    newRound() {
      this.sequence.push(this.randomNumber());
      this.copy = this.sequence.slice(0);
    },
    randomNumber() {
      return Math.floor((Math.random()*4)+1);
    },
    registerClick(e) {
      const desiredResponse = this.copy.shift();
      const actualResponse = $(e.target).data('tile');
      this.active = (desiredResponse === actualResponse);
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
    endGame() {
      $('p[data-action=lose]').show();
      $($('[data-round]').get(0)).text('0');
    },
    activateSimonBoard(e) {
      const id = e.target.getAttribute('data-id')
    },
  }
}
</script>

<style lang="scss">
.panel-wrapper {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border-radius: 100%;
  position: relative;
}
.panel {
  display: inline-block;
  width: 150px;
  height: 150px;
  cursor: pointer;
}
.panel:hover {
  box-shadow: inset 0 0 20px 5px cyan;
}
.bottom-right-panel {
  border-bottom-right-radius: 100%;
  background-color: blue;
  opacity: 0.6;
}
.bottom-left-panel {
  border-bottom-left-radius: 100%;
  background-color: yellow;
  opacity: 0.6;
}
.top-right-panel {
  border-top-right-radius: 100%;
  background-color: red;  
  opacity: 0.6;
}
.top-left-panel {
  border-top-left-radius: 100%;
  background-color: green;
  opacity: 0.6;
}
.panel:active {
  opacity: 1;
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
  justify-content: center;
  align-items: center;
  width: 100px;
  height: 100px;
  border-radius: 50%;
  background: white;
  position: absolute;
}
</style>