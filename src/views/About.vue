<template>
  <div class="about container">
    <section class="row">
      <div class="col-sm-6">
        <h1 class="text-center">YOU</h1>
        <div class="healthbar">
          <div class="healthbar text-center" style="background-color: green; margin: 0; color: white;" :style="{width: monsterHealth + '%'}">
            {{ playerHealth }}
          </div>
        </div>
      </div>

       <div class="col-sm-6">
            <h1 class="text-center">MONSTER</h1>
            <div class="healthbar">
                <div class="healthbar text-center" style="background-color: green; margin: 0; color: white;" :style="{width: playerHealth + '%'}">
                  {{ monsterHealth }}
                </div>
            </div>
        </div>
    </section>

    <section class="row controls" v-if="!gameIsRunning">
      <div class="col-sm-12">
        <button id="start-game" class="btn btn-success text-dark" @click="startGame" >START NEW GAME</button>
      </div>
    </section>
    <section class="row controls" v-else>
      <div class="col-sm-12">
        <button id="attack" class="btn" @click="attack">ATTACK</button>
        <button id="special-attack" class="btn" @click="specialAttack">SPECIAL ATTACK</button>
        <button id="heal" class="btn" @click="heal">HEAL</button>
        <button id="give-up" class="btn bg-info" @click="giveUp">GIVE UP</button>
      </div>
    </section>
    <section v-if="turns.length > 0">
      <div class="row log container">
        <div class="col-sm-12">
          <ul>

            <li v-for="item in turns" :key="item">
              {{item.text}}
            </li>

          </ul>
        </div>
      </div>
    </section>
  </div>
</template>



<script>

export default {
  name: 'About',
  data (){
    return{
      playerHealth:100,
      monsterHealth:100,
      // to set game to new game
      gameIsRunning:false,
      turns: []
    }
  },
  methods:{
    startGame(){
      this.gameIsRunning = true;
      this.playerHealth = 100;
      this.monsterHealth = 100;
      this.turns = [];
    },
    attack(){
      // for monster
      var damage = this.calculateDamage(3,10);
       this.monsterHealth -= damage
       this.turns.unshift({
         isPlayer: true,
         text: "Monster hits player for" + " " + damage
       });
          if (this.checkWin()) {
            return
          }

        // for player
       this.playerHealth -= this.calculateDamage(5,12); 
       this.checkWin();

    },
    specialAttack(){
         this.monsterHealth -= this.calculateDamage(3,10);
          if (this.checkWin()) {
            return
          }
          this.monsterAttacks();
    },
    heal(){
      if (this.playerHealth <= 90) {
        this.playerHealth += 100
      }else{
        this.playerHealth = 100;
      }
      this.turns.unshift({
         isPlayer: true,
         text: "Player heals for 10"
       });

      this.monsterAttacks();  
    },
    giveUp(){
      this.gameIsRunning = false;
    },
    monsterAttacks(){
      var damage = this.calculateDamage(5,12);
      this.playerHealth -= damage;
      this.checkWin() ;
      this.turns.unshift({
         isPlayer: true,
         text: "Player hits monster hard for" + " " + damage
       });
    },
    calculateDamage( min, max){
      return Math.max(Math.floor(Math.random() * max) + 1, min);
    },
    checkWin(){

       if (this.monsterHealth <= 0){
         if (confirm("You've won! Start new game?")) {
           this.startGame();
         }else{
           this.gameIsRunning = false;
         }
         return true;
       }else if (this.playerHealth <= 0) {
         if (confirm("You've lost! Start new game?")) {
           this.startGame();
         }else{
           this.gameIsRunning = false;
         }
         return true;
       }
       return false;
    }
  }
  
}
</script>

<style scoped>
  .text-center {
    text-align: center;
}

.healthbar {
    width: 80%;
    height: 40px;
    background-color: #eee;
    margin: auto;
    transition: width 500ms;
}

.controls, .log {
    margin-top: 30px;
    text-align: center;
    padding: 10px;
    border: 1px solid #ccc;
    box-shadow: 0px 3px 6px #ccc;
}

.turn {
    margin-top: 20px;
    margin-bottom: 20px;
    font-weight: bold;
    font-size: 22px;
}

.log ul {
    list-style: none;
    font-weight: bold;
    text-transform: uppercase;
}

.log ul li {
    margin: 5px;
}

.log ul .player-turn {
    color: blue;
    background-color:blue;
}

.log ul .monster-turn {
    color: red;
    background-color: #ffc0c1;
}

button {
    font-size: 20px;
    background-color: #eee;
    padding: 12px;
    box-shadow: 0 1px 1px black;
    margin: 10px;
}

#start-game {
    background-color: #aaffb0;
}

#start-game:hover {
    background-color:green;
    color: white;
}

#attack {
    background-color: #ff7367;
}

#attack:hover {
    background-color: #ff3f43;
}

#special-attack {
    background-color: #ffaf4f;
}

#special-attack:hover {
    background-color: #ff9a2b;
}

#heal {
    background-color: #aaffb0;
}

#heal:hover {
    background-color: #76ff7e;
}

#give-up {
    background-color: #ffffff;
}

#give-up:hover {
    background-color: #c7c7c7;
}
</style>
