<template>
  <div class="bg-gray-700 text-white text-center min-h-screen flex flex-col">
    <header class="container mx-auto p-6">
      <h1 class="text-4xl font-bold">Rock, Paper, Scissors</h1>
    </header>
    <main class="container mx-auto p-6 flex-1">
      <div
        v-if="choice === null"
        class="flex items-center justify-center -mx-6"
      >
        <button
          @click="play('rock')"
          class="bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-gray-400"
        >
          <img src="./assets/rock.svg" alt="rock" class="w-full" />
        </button>

        <button
          @click="play('paper')"
          class="bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-gray-400"
        >
          <img src="./assets/paper.svg" alt="paper" class="w-full" />
        </button>

        <button
          @click="play('scissors')"
          class="bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-gray-400"
        >
          <img src="./assets/scissor.svg" alt="scissor" class="w-full" />
        </button>
      </div>
      <div v-else>
        <div class="text-3xl mb-4">
          You Picked:
          <span class="font-bold uppercase">{{ choice }}</span>
        </div>

        <div class="text-3xl mb-4">
          Computer Picked:
          <span class="font-bold uppercase">{{ compChoice }}</span>
        </div>

        <div class="text-6xl mb-12">
          {{ verdict }}
        </div>

        <button @click="resetGame" class="bg-gray-400 text-lg py-2 px-4">
          Reset
        </button>
      </div>
      <div class="mt-12 text-3xl mb-4">
        {{ wins }} : {{ draw }} : {{ losses }}
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      wins: 0,
      losses: 0,
      draw: 0,
      choice: null,
      compChoice: null,
      verdict: null,
      outcomes: {
        rock: {
          rock: "draw",
          paper: "loss",
          scissors: "win",
        },
        paper: {
          rock: "win",
          paper: "draw",
          scissors: "loss",
        },
        scissors: {
          rock: "loss",
          paper: "win",
          scissors: "draw",
        },
      },
    };
  },
  computed: {
    winpercentage() {
      const total = this.wins + this.losses + this.draw;
      return total ? (this.wins / total) * 100 : 0;
    },
  },
  methods: {
    play(c) {
      this.choice = c;
      let choices = ["rock", "paper", "scissors"];
      let random = Math.floor(Math.random() * choices.length);
      this.compChoice = choices[random];
      const outcome = this.outcomes[this.choice][this.compChoice];

      if (outcome === "win") {
        this.wins++;
        this.verdict = "You win!";
      } else if (outcome === "loss") {
        this.losses++;
        this.verdict = "You lose!";
      } else {
        this.draw++;
        this.verdict = "Draw!";
      }
      this.savegame();
    },
    savegame() {
      localStorage.setItem("wins", this.wins);
      localStorage.setItem("losses", this.losses);
      localStorage.setItem("draw", this.draw);
    },
    loadgame() {
      this.wins = localStorage.getItem("wins");
      this.losses = localStorage.getItem("losses");
      this.draw = localStorage.getItem("draw");
    },
    resetGame() {
      this.choice = null;
      this.compChoice = null;
      this.verdict = null;
    },
  },
  mounted() {
    this.loadgame();

    window.addEventListener("keypress", (e) => {
      if (e.key === "r") {
        this.resetGame();
      }
    });
  },
};
</script>

<style></style>
