<template>
    <div id="app">
        <div v-if="!lost && !won">
            <div class="row">
                <div class="col-xs-4">
                    <pre>Rates : {{rates.length}}</pre>
                    <pre style="color:red" v-if="!user">Try: {{letter}}</pre>
                </div>
                <div class="col-xs-8">
                    <pre v-if="rates.length === 0">







          </pre>
                    <pre v-if="rates.length === 1">






------+
          </pre>
                    <pre v-if="rates.length === 2">

      |
      |
      |
      |
      |
------+
          </pre>
                    <pre v-if="rates.length === 3">
  +---+
      |
      |
      |
      |
      |
------+
          </pre>
                    <pre v-if="rates.length === 4">
  +---+
  |   |
      |
      |
      |
      |
------+
          </pre>
                    <pre v-if="rates.length === 5">
  +---+
  |   |
  O   |
      |
      |
      |
------+
          </pre>
                    <pre v-if="rates.length === 6">
  +---+
  |   |
  O   |
  |   |
      |
      |
------+
          </pre>
                    <pre v-if="rates.length === 7">
  +---+
  |   |
  O   |
 /|   |
      |
      |
------+
          </pre>
                    <pre v-if="rates.length === 8">
  +---+
  |   |
  O   |
 /|\  |
      |
      |
------+
          </pre>
                    <pre v-if="rates.length === 9">
  +---+
  |   |
  O   |
 /|\  |
 /    |
      |
------+
          </pre>
                    <pre v-if="rates.length === 10">
  +---+
  |   |
  O   |
 /|\  |
 / \  |
      |
------+
          </pre>
                </div>
            </div>

            <button @click="role('user')">User</button>
            Or
            <button @click="role('comp')">Computer</button>
            <pre class="gros">
        <span v-for="l in wordToDisplay">{{l}} </span>
      </pre>
            <div v-if="user">
                <button class="btn btn-default" v-for="l in letters" @click="click(l)"
                        :disabled="lettersChosen.indexOf(l) > -1">{{l}}
                </button>
            </div>
            <div v-if="!user">
                <input type="text" v-model="answer">
                <button @click="setAnswer()">Submit</button>
            </div>
        </div>
        <div v-if="lost" class="danger">
      <pre>
  +---+
  |   |
  O   |
 /|\  |
 / \  |
      |
------+
          </pre>
            You lost<br>
            <button @click="reset" class="btn btn-danger">Try again</button>
        </div>
        <div v-if="won" class="success">
            You have won<br>
            <button @click="reset" class="btn btn-success">Try again</button>
        </div>
    </div>
</template>

<script>

    const words = ['snake', 'dog', 'bird', 'horse'];

    export default {
        name: 'app',
        data() {
            return {
                letters: ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z'],
                lettersChosen: [],
                rates: [],
                successful: [],
                against: this.wordAtRandom(),
                lost: false,
                won: false,
                user: true,
                answer: '',
                letter: '',
            }
        },
        computed: {
            wordToDisplay() {
                const against = this.against;
                let sort = '';
                for (const l of against) {
                    sort += this.successful.indexOf(l) > -1 ? l : '_';
                }
                return sort;
            }
        },
        methods: {
            click(letter) {
                this.lettersChosen.push(letter);
                if (this.against.indexOf(letter) > -1) {
                    this.successful.push(letter);
                } else {
                    this.rates.push(letter);
                }
                this.check();
            },
            check() {
                this.lost = this.rates.length >= 10;
                this.won = this.wordToDisplay === this.against;
            },
            reset() {
                this.lettersChosen = [];
                this.rates = [];
                this.successful = [];
                this.against = '';
                this.lost = false;
                this.won = false;
                this.against = this.wordAtRandom();
                clearInterval(this.interval);
            },
            wordAtRandom() {
                return words[Math.floor(Math.random() * words.length)];
            },
            lettersAtRandom(){
                return this.letters[Math.floor(Math.random() * this.letters.length)];
            },
            role(role){
                if(role === 'user'){
                    this.user = true;
                }else{
                    this.user = false;
                }
            },
            setAnswer(){
                this.against = this.answer;
                this.repeater()
            },
            repeater (){
                 self = this;
                 let callCount = 1;
                    this.interval = setInterval(function () {
                     if (self.rates.length < 10 ) {
                         self.letter = self.lettersAtRandom();
                         callCount += 1;
                         self.click(self.letter);
                         console.log(self.letter);
                     } else {
                         clearInterval(self.repeater);
                     }
                 }, 2000);
             }
        }
    }
</script>
<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
    .success {
        color: green
    }
    .danger {
        color: red;
    }
</style>
