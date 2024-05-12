<script>
import red from "./sounds/1.mp3";
import blue from "./sounds/2.mp3";
import yellow from "./sounds/3.mp3";
import green from "./sounds/4.mp3";

// making feature

export default {
  data() {
    return {
      colors: ["red", "green", "blue", "yellow"],
      sequence: [],
      timing: 300,
      currentSequence: [],
      text: "",
      canclick: false,
    };
  },
  methods: {
    startButton() {
      this.text = "";
      this.resetGame();
      setTimeout(() => {
        this.generateColor();
      }, 500);
    },
    async clicked(color) {
      if (this.currentSequence.length == 0 || this.canclick == true) {
        return;
      }
      await this.changeColor(color);
      if (color == this.currentSequence[0]) {
        this.currentSequence.shift();
        if (this.currentSequence.length == 0) {
          await setTimeout(() => {
            this.generateColor();
          }, 300);
        }
      } else {
        this.text = "Sorry, you lost!";
        this.sequence = [];
        this.currentSequence = [];
        this.canclick = false;
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
        this.canclick = true;
        document.getElementById(color).className += " active";
        switch (color) {
          case "red":
            new Audio(red).play();
            break;
          case "blue":
            new Audio(blue).play();
            break;
          case "yellow":
            new Audio(yellow).play();
            break;
          default:
            new Audio(green).play();
        }

        setTimeout(() => {
          document.getElementById(color).className = `circle ${color}`;
          this.canclick = false;
          setTimeout(() => resolve(), 100);
        }, this.timing);
      });
    },
    resetGame() {
      this.sequence = [];

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
      <div class="level">Current level is {{ sequence.length }}</div>
      <div>
        <button @click="startButton">Start</button>
      </div>
      <p>Game options:</p>
      <p style="margin: 1vh 0 1vh 0">Time between lights (in ms):</p>
      <div>
        <input
          type="radio"
          name="difficulty"
          value="300"
          @change="changeDifficulty"
          checked
        />
        400
      </div>

      <div>
        <input
          type="radio"
          name="difficulty"
          value="900"
          @change="changeDifficulty"
        />
        1000
      </div>

      <div>
        <input
          type="radio"
          name="difficulty"
          value="1400"
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
  font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
    "Lucida Sans Unicode", Geneva, Verdana, sans-serif;
  color: white;
  background-color: rgb(59, 59, 59);
}
.container {
  margin: auto;
  width: 90vw;
  height: 100vh;
  background-color: rgb(59, 59, 59);
  padding: 5vw;
  border-radius: 20px;
}
.menu {
  display: flex;
  flex-direction: column;
  margin-left: 44vw;
  margin-top: 7vw;
}
.menu .level {
  font-size: 30px;
  font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
    "Lucida Sans Unicode", Geneva, Verdana, sans-serif;
}
.menu button {
  margin-top: 1vw;
  width: 6vw;
  height: 3vw;
  background-color: black;
  border: 0;
  border-radius: 10px;
  margin-right: 1vw;
  font-size: 20px;
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
  display: block;
  cursor: pointer;
  position: absolute;
  opacity: 0.6;
  transition: 0.2s;
}
.blue {
  background-color: dodgerblue;
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
    clip: rect(0vw, 30vw, 30vw, 0vw);
  }
  .red {
    clip: rect(0vw, 60vw, 30vw, 30vw);
  }
  .green {
    clip: rect(30vw, 60vw, 60vw, 30vw);
  }
  .yellow {
    clip: rect(30vw, 30vw, 60vw, 0vw);
  }
  .menu {
    margin-top: 40vh;
    margin-left: 25vw;
  }
  .menu button {
    width: 24vw;
    height: 6vh;
  }
}
</style>
