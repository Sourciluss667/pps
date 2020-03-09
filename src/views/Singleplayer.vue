<template>
  <div class="singleplayer">
    <h1>Choose your card ?</h1>

    <h2 style="color: white;"><span style="color: #bf00c5">{{ pScore }}</span> - <span style="color: #FF0000;">{{ bScore }}</span></h2>

    <div v-if="pCard.length == 0">
        <h2 style="color: white;"><span>Winner is : </span><span v-if="pScore > bScore" style="color: #00FF00;">PLAYER</span><span v-if="pScore < bScore" style="color: #FF0000;">BOT</span><span v-if="pScore == bScore" style="color: #FFFF00;">EGALITY</span></h2>
    </div>

    <div class="cards">
        <img :src="cards[item]" :alt="item" v-for="(item, index) in pCard" :key="index" class="card" v-on:click="chooseCard(index)">
    </div>

    <div class="resultsHistory">
        <span style="color: #bf00c5;">History</span>
        <div v-for="(line, index) in history" :key="index">
            <div v-if="index == 0" style="color: #00FF00;" class="roundHistory"><span style="position: relative; top: 20%;">First Turn</span></div>
            <div v-if="index == 1" style="color: #FFFF33;" class="roundHistory"><span style="position: relative; top: 20%;">Second Turn</span></div>
            <div v-if="index == 2" style="color: #FF8C00;" class="roundHistory"><span style="position: relative; top: 20%;">Third Turn</span></div>

            <div>
                <img :src="cards[line.pString]" :alt="line.pString" class="cardHistory">
                <span style="color: #00FF00;" class="separatorHistory" v-if="line.win == 'player'"> - W - </span>
                <span style="color: #FF0000;" class="separatorHistory" v-if="line.win == 'bot'"> - L - </span>
                <span style="color: #FFFF00;" class="separatorHistory" v-if="line.win == 'egality'"> - E - </span>
                <img :src="cards[line.bString]" :alt="line.pBot" class="cardHistory">
            </div>
        </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
export default {
  name: 'Singleplayer',
  components: {},
  data () {
    return {
      pCard: [],
      bCard: [],
      pScore: 0,
      bScore: 0,
      history: [],
      cards: {}
    }
  },
  methods: {
    startNewGame () {
      for (let i = 0; i < 3; i++) {
        this.pCard[i] = this.getRandomCard()
        this.bCard[i] = this.getRandomCard()
      }
    },
    getRandomCard () {
      const cards = ['rock', 'paper', 'scissors']
      return cards[Math.floor(Math.random() * Math.floor(3))]
    },
    getImgUrl (pic) {
      return require(`@/assets/${pic}.jpg`)
    },
    chooseCard (index) {
      this.nextTurn(this.pCard[index], this.botChooseCard())
      this.pCard.splice(index, 1)
    },
    botChooseCard () {
      const choose = Math.floor(Math.random() * Math.floor(this.bCard.length))
      const result = this.bCard[choose]
      this.bCard.splice(choose, 1)
      return result
    },
    nextTurn (pString, bString) {
      let win = ''
      const p = this.reverseCard(pString)
      const b = this.reverseCard(bString)
      if (p === 0) { // rock
        if (b === 0) { // egality
          win = 'egality'
        } else if (b === 1) { // bot win
          win = 'bot'
        } else if (b === 2) { // player win
          win = 'player'
        }
      } else if (p === 1) { // paper
        if (b === 1) { // egality
          win = 'egality'
        } else if (b === 2) { // bot win
          win = 'bot'
        } else if (b === 0) { // player win
          win = 'player'
        }
      } else if (p === 2) { // rock
        if (b === 2) { // egality
          win = 'egality'
        } else if (b === 0) { // bot win
          win = 'bot'
        } else if (b === 1) { // player win
          win = 'player'
        }
      } else {
        win = 'error'
      }

      if (win === 'player') this.pScore++
      else if (win === 'bot') this.bScore++

      this.history.push({ pString, bString, p, b, win })
    },
    reverseCard (str) {
      switch (str) {
        case 'rock':
          return 0
        case 'paper':
          return 1
        case 'scissors':
          return 2
      }
    }
  },
  created () {
    this.cards = {
      rock: this.getImgUrl('rock'),
      paper: this.getImgUrl('paper'),
      scissors: this.getImgUrl('scissors')
    }
    this.startNewGame()
  }
}
</script>

<style scoped>
h1 {
    color: white;
}

ul {
    position: relative;
    left: -60px;
}

li {
    list-style: none;
    color: white;
}

.cards {
    display: block;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}

.card {
    width: 256px;
    height: 355px;
    cursor: pointer;
    display: inline-block;
    margin-left: 50px;
    border: rgba(0, 0, 0, 0) 1px solid;
}

.card:hover {
    border: purple 1px solid;
}

.cardHistory {
    cursor: pointer;
    width: 60px;
    height: 100px;
}

.resultsHistory {
    position: absolute;
    display: block;
    width: 200px;
    top: 5px;
    left: 5px;
}

.separatorHistory {
    display: inline-block;
    position: relative;
    margin-left: 10px;
    margin-right: 10px;
    top: -50px;
    transform: translateY(50%);
}

.roundHistory {
    display: inline-block;
    margin-top: 5px;
    margin-bottom: 5px;
    background-color: #303030;
    height: 25px;
    width: 100%;
}
</style>
