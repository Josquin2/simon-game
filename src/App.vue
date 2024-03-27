<script>
export default {
  data() {
    return {
      colors: ["red", "green", "blue", "yellow"],
      sequence: [],
      userSequence: [],
      timing: 400,
      currentSequence: [],
      text: "",
      canclick: 0,
    };
  },
  methods: {
    startButton() {
      this.resetGame();
      setTimeout(() => {
        this.generateColor();
      }, 500);
    },
    async clicked(color) {
      if (this.currentSequence.length == 0 || this.canclick > 0) {
        return;
      }
      // console.log(this.currentSequence);
      // console.log(this.currentSequence.length);
      await this.changeColor(color);
      if (color == this.currentSequence[0]) {
        console.log(color);
        this.currentSequence.shift();
        if (this.currentSequence.length == 0) {
          await this.generateColor();
        }
      } else {
        this.text = "You loose!";

        this.resetGame();
        setTimeout(() => (this.text = ""), 1500);
      }
    },
    changeDifficulty(e) {
      this.timing = e.target.value;
    },
    async generateColor() {
      const randomColor =
        this.colors[Math.floor(Math.random() * this.colors.length)];
      this.sequence.push(randomColor);
      for (let item in this.sequence) {
        this.currentSequence.push(this.sequence[item]);
      }
      for (let i = 0; i < this.sequence.length; i++) {
        await this.changeColor(this.sequence[i]);
      }
    },
    changeColor(color) {
      return new Promise((resolve) => {
        this.canclick = 1;
        document.getElementById(color).className += " active";
        setTimeout(() => {
          document.getElementById(color).className = `circle ${color}`;
          this.canclick = 0;
          setTimeout(() => resolve(), 200);
        }, this.timing);
      });
    },
    resetGame() {
      this.sequence = [];
      this.userSequence = [];
      this.currentSequence = [];
    },
  },
};
</script>

<template>
  <div class="container">
    <div class="foursome">
      <span id="blue" @click="clicked('blue')" class="circle blue"></span>
      <span id="red" @click="clicked('red')" class="circle red"></span>
      <span id="green" @click="clicked('green')" class="circle green"></span>
      <span id="yellow" @click="clicked('yellow')" class="circle yellow"></span>
    </div>
    <div class="menu">
      <div class="level">Level {{ sequence.length }}</div>
      <div>
        <button @click="startButton">Start</button>
      </div>
      <p>Game options</p>
      <div>
        <input
          type="radio"
          name="difficulty"
          value="400"
          @change="changeDifficulty"
          checked
        />
        400
      </div>

      <div>
        <input
          type="radio"
          name="difficulty"
          value="1000"
          @change="changeDifficulty"
        />
        1000
      </div>

      <div>
        <input
          type="radio"
          name="difficulty"
          value="1500"
          @change="changeDifficulty"
        />
        1500
      </div>

      <p>{{ text }}</p>
    </div>
  </div>
</template>
<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.container {
  margin: auto;
  width: 90vw;
  height: 100vh;
  background-color: rgb(225, 225, 225);
  padding: 5vw;
  border-radius: 20px;
}
.menu {
  display: flex;
  flex-direction: column;
  margin-left: 34vw;
}
.menu .level {
  font-size: 30px;
  font-family: Arial, Helvetica, sans-serif;
}
.menu button {
  margin-top: 1vw;
  width: 6vw;
  height: 3vw;
  background-color: white;
  border: 0;
  border-radius: 10px;
  margin-right: 1vw;
}
.menu p {
  font-size: 20px;
  font-family: Arial, Helvetica, sans-serif;
  margin-top: 1vw;
}
.circle {
  height: 30vw;
  width: 30vw;
  border-radius: 50%;
  background-color: aliceblue;
  display: block;
  cursor: pointer;
  position: absolute;
  opacity: 0.6;
  transition: 0.2s;
}
.blue {
  background-color: aqua;
  clip: rect(0vw, 15vw, 15vw, 0vw);
}
.red {
  background-color: red;
  clip: rect(0vw, 30vw, 15vw, 15vw);
}
.green {
  background-color: green;
  clip: rect(15vw, 30vw, 30vw, 15vw);
}
.yellow {
  background-color: yellow;
  clip: rect(15vw, 15vw, 30vw, 0vw);
}
.active {
  opacity: 100%;
}
@media only screen and (max-width: 600px) {
  .circle {
    width: 60vw;
    height: 60vw;
    margin-left: 8vw;
  }
  .blue {
    background-color: aqua;
    clip: rect(0vw, 30vw, 30vw, 0vw);
  }
  .red {
    background-color: red;
    clip: rect(0vw, 60vw, 30vw, 30vw);
  }
  .green {
    background-color: green;
    clip: rect(30vw, 60vw, 60vw, 30vw);
  }
  .yellow {
    background-color: yellow;
    clip: rect(30vw, 30vw, 60vw, 0vw);
  }
  .menu {
    margin-top: 40vh;
    margin-left: 27vw;
  }
  .menu button {
    width: 16vw;
    height: 4vh;
  }
}
</style>
