<template>
  <v-app>
    <v-container>
      <div class="section">
        <v-row class="text-center">
          <v-col cols="6">
            <h2>You</h2>
          </v-col>
          <v-col cols="6">
            <h2>Monster</h2>
          </v-col>
        </v-row>
        <v-row class="text-center">
          <v-col>
            <div class="healthBar">
              <div class="actualHealthBar" :style="{width: playerHealth + '%'}">
                {{ playerHealth }}
              </div>
            </div>
          </v-col>
          <v-col>
            <div class="healthBar">
              <div class="actualHealthBar" :style="{width: monsterHealth + '%'}">
                {{ monsterHealth }}
              </div>
            </div>
          </v-col>
        </v-row>
      </div>
      <div class="section" v-if="!gameIsRunning">
        <v-row class="text-center">
          <v-col>
            <v-btn color="success" id="startGame" @click="startGame">Start New Game</v-btn>
          </v-col>
        </v-row>
      </div>
      <div class="section" v-else>
        <v-row class="text-center">
          <v-col>
            <v-btn id="attack" @click="attack">Attack</v-btn>
          </v-col>
          <v-col>
            <v-btn color="primary" id="specialAttack" @click="specialAttack">Special Attack</v-btn>
          </v-col>
          <v-col>
            <v-btn color="success" id="heal" @click="heal">Heal</v-btn>
          </v-col>
          <v-col>
            <v-btn color="error" id="surrender" @click="surrender">Surrender</v-btn>
          </v-col>
        </v-row>
      </div>
      <div class="section" v-if="turns.length > 0">
        <v-row class="text-center">
          <v-col>
            <ul>
              <li v-for="turn in turns"
                v-bind:key="turn"
                :class="{'player-turn': turn.isPlayer, 'monster-turn': !turn.isPlayer}">
                {{ turn.text }}
              </li>
            </ul>
          </v-col>
        </v-row>
      </div>
    </v-container>
  </v-app>
</template>

<script>
  export default {
    name: "App",
    components: {

    },
    data: () => ({
      playerHealth: 100,
      monsterHealth: 100,
      gameIsRunning: false,
      turns: []
    }),
    methods: {
      startGame() {
        this.gameIsRunning = true;
        this.playerHealth = 100;
        this.monsterHealth = 100;
        this.turns = [];
        this.turns.unshift({
          isPlayer: false,
          text: 'New game starting....'
        });
      },
      attack() {
        this.playerAttack(3, 10);

        if (this.checkGameEnded()) {
          return;
        }

        this.monsterAttack();
      },
      specialAttack() {
        this.playerAttack(10, 20);

        if (this.checkGameEnded()) {
          return;
        }

        this.monsterAttack();
      },
      heal() {
        if (this.playerHealth <= 90) {
          this.playerHealth += 10;
        } else {
          this.playerHealth = 100;
        }
        this.turns.unshift({
          isPlayer: true,
          text: 'HP Increased to ' + this.playerHealth
        });
        this.monsterAttack();
      },
      surrender() {
        if (confirm('You surrendered! New game?')) {
          this.startGame();
        } else {
          this.gameEnded();
        }
      },
      monsterAttack() {
        let damage = this.rngCalculator(5, 12);
        this.playerHealth -= damage;
        this.checkGameEnded();
        this.turns.unshift({
          isPlayer: false,
          text: 'Monster hits Player for ' + damage
        })
      },
      playerAttack(min, max) {
        let damage = this.rngCalculator(min, max);
        this.monsterHealth -= damage;
        this.turns.unshift({
          isPlayer: true,
          text: 'Player hits Monster for ' + damage
        });
      },
      rngCalculator(min, max) {
        return Math.max(Math.floor(Math.random() * max) + 1, min);
      },
      checkGameEnded() {
        if (this.monsterHealth <= 0) {
          this.turns.unshift({
            isPlayer: true,
            text: 'You have won! ^-^'
          });

          if (confirm('You won! New game?')) {
            this.startGame();
          } else {
            this.gameEnded();
          }
          return true;
        } else if (this.playerHealth <= 0) {
          this.turns.unshift({
            isPlayer: false,
            text: 'You have lost! T-T'
          });
          if (confirm('You lost! New game?')) {
            this.startGame();
          } else {
            this.gameEnded();
          }
          return true;
        }

        return false;
      },
      gameEnded() {
        this.gameIsRunning = false;
      }
    }
  };
</script>

<style>
  .section {
    border: 1px black solid;
    padding-top: 10px;
    padding-bottom: 10px;
    margin-bottom: 50px;
    box-shadow: 3px 3px #888888;

  }

  .healthBar, .actualHealthBar {
    background-color: #eee;
    height: 40px;
    width: 80%;
    margin: auto;
    transition: width 500ms;
  }

  .actualHealthBar {
    background-color: green;
    margin: 0;
    color: white;
  }

  ul {
    list-style-type:none;
  }

  .player-turn {
    color: blue;
    background-color: #e4e8ff;
  }
  .monster-turn {
    color: red;
    background-color: #ffc0c1;
  }
</style>