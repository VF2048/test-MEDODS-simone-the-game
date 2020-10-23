<template>
  <div id="app">
    <div class="container">
      <h1>Simon Says</h1>
      <div class="simon">
        <ul>
          <li
            class="blue"
            data-tile="1"
            v-on:click="select(0)"
            v-bind:class="{ active: isActive0 }"
          ></li>
          <li
            class="red"
            data-tile="2"
            v-on:click="select(1)"
            v-bind:class="{ active: isActive1 }"
          ></li>
          <li
            class="yellow"
            data-tile="3"
            v-on:click="select(2)"
            v-bind:class="{ active: isActive2 }"
          ></li>
          <li
            class="green"
            data-tile="4"
            v-on:click="select(3)"
            v-bind:class="{ active: isActive3 }"
          ></li>
        </ul>
      </div>
      <div class="game-info">
        <h2>
          Round: <span data-round="0">{{ score }}</span>
        </h2>
        <button data-action="start" v-on:click="start()">Start</button>
        
      </div>
      <div class="game-options" v-on:click="restart()">
        <h2>Game Options:</h2>
        <input
          type="radio"
          name="mode"
          v-model="time"
          v-bind:value="1500"
          checked=""
        />Легкий<br />
        <input
          type="radio"
          name="mode"
          v-model="time"
          v-bind:value="1000"
        />Средний<br />
        <input
          type="radio"
          name="mode"
          v-model="time"
          v-bind:value="400"
        />Сложный<br />
      </div>
    </div>
    <dir class="loss" v-if="loss">
      <dir class="loss-box">
        <strong class="loss-text">LOST</strong>
      </dir>
    </dir>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      audio: [
        new Audio(
          "https://raw.githubusercontent.com/kellyk/javascript-simon/master/sounds/1.mp3"
        ),
        new Audio(
          "https://raw.githubusercontent.com/kellyk/javascript-simon/master/sounds/2.mp3"
        ),
        new Audio(
          "https://raw.githubusercontent.com/kellyk/javascript-simon/master/sounds/3.mp3"
        ),
        new Audio(
          "https://raw.githubusercontent.com/kellyk/javascript-simon/master/sounds/4.mp3"
        ),
      ],
      isActive: [false, false, false, false],
      isActive0: false,
      isActive1: false,
      isActive2: false,
      isActive3: false,
      time: 1500,
      notes: [],
      playing: false,
      score: 0,
      loss: false,
    };
  },
  methods: {
    start() {
      this.notes = [];
      for (let i = 0; i <= this.score; i++) {
        this.notes.push(Math.floor(Math.random() * 4));
      }
      this.play(this.notes, this.score);
    },
    play(notes, i) {
      this.song(notes[i], this.time / 2);
      if (i < 1) {
        this.playing = true;
        return;
      } else i -= 1;
      setTimeout(() => this.play(notes, i), this.time);
    },
    select(button) {
      if (this.playing) {
        this.song(button, 100);
        if (this.notes.pop() == button) {
          if (this.notes.length < 1) {
            this.playing = false;
            setTimeout(() => this.start(), this.time);
            this.score += 1;
          }
        } else {
          this.restart();
          this.loss = true;
          setTimeout(() => (this.loss = false), this.time / 3);
        }
      }
    },
    restart() {
      this.notes = [];
      this.score = 0;
      this.playing = false;
    },
    song(songs, time) {
      this.isActive[songs] = true;
      this.audio[songs].play();
      this.reactivity();
      setTimeout(() => {
        this.isActive[songs] = false;
        this.reactivity();
      }, time / 2);
    },
    reactivity() {
      this.isActive0 = this.isActive[0];
      this.isActive1 = this.isActive[1];
      this.isActive2 = this.isActive[2];
      this.isActive3 = this.isActive[3];
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
body {
  font-family: Arial, serif;
  color: #333;
  -webkit-user-select: none; /* Chrome/Safari */
  -moz-user-select: none; /* Firefox */
  -ms-user-select: none; /* IE10+ */
  -o-user-select: none;
  user-select: none;
}

h1 {
  margin: 1em 0 2em;
  text-align: center;
}

ul {
  list-style: none;
}

ul,
li {
  padding: 0;
  margin: 0;
}

p[data-action="lose"] {
  display: none;
}

.active {
  opacity: 1 !important;
}

.clearfix {
  width: 100%;
  clear: both;
}

.container {
  width: 540px;
  margin: 0 auto;
}

.simon {
  background: #fff;
  position: relative;
  float: left;
  margin-right: 3em;
  width: 302px;
  height: 295px;
  -webkit-border-radius: 150px 150px 150px 150px;
  border-radius: 150px 150px 150px 150px;
  -moz-box-shadow: 2px 1px 12px #aaa;
  -webkit-box-shadow: 2px 1px 12px #aaa;
  -o-box-shadow: 2px 1px 12px #aaa;
  box-shadow: 2px 1px 12px #aaa;
}

.red,
.blue,
.yellow,
.green {
  opacity: 0.6;
  height: 290px;
  -webkit-border-radius: 150px 150px 150px 150px;
  border-radius: 150px 150px 150px 150px;
  position: absolute;
  text-indent: 10000px;
}

.red:hover,
.blue:hover,
.yellow:hover,
.green:hover {
  border: 2px solid black;
}

.red {
  background: #ff5643;
  clip: rect(0px, 300px, 150px, 150px);
  width: 296px;
}

.blue {
  background: dodgerblue;
  clip: rect(0px, 150px, 150px, 0px);
  width: 300px;
}

.yellow {
  background: #feef33;
  clip: rect(150px, 150px, 300px, 0px);
  width: 300px;
}

.green {
  background: #bede15;
  clip: rect(150px, 300px, 300px, 150px);
  width: 296px;
}

.game-info {
  margin-top: 90px;
}

.game-info button {
  width: 5em;
  box-sizing: border-box;
  font-size: 1.4em;
  -webkit-border-radius: 10px 10px 10px 10px;
  border-radius: 10px 10px 10px 10px;
  background: #6dabe8;
  border: none;
  padding: 0.3em 0.6em;
}

.game-info button:hover {
  background: #78bcff;
}

.game-options h2 {
  margin-top: 30px;
  margin-bottom: 0;
}

.game-options input[type="radio"] {
  margin-right: 10px;
}

.hoverable:hover {
  cursor: pointer;
}
.loss {
  position: absolute;
  top: 0;
  left: 0;
  height: 55%;
  width: 100%;
  background: #7d7777a6;
}
.loss-box {
  margin-top: 28%;
  left: 44%;
}
.loss-text {
  color: red;
  font-size: 500%;
}
</style>
