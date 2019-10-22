<template>
    <div id="app">
        <div v-if="!lost && !won">
            <div class="row">
                <div class="col-xs-4">
                    <pre>Rates : {{rates.length}}</pre>
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
    import axios from 'axios';

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
                user: false,
                answer: '',

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
            },
            wordAtRandom() {
                return words[Math.floor(Math.random() * words.length)];
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
                this.checkWorld();
            },
            checkWorld(){
                // const url = 'https://od-api.oxforddictionaries.com:443/api/v2/entries/en-gb/'+ this.answer.toLowerCase();
                // const options = {
                //     method: 'GET',
                //     headers: {
                //         'app_id': 'dc4282d6',
                //         'app_key': '281d675436705fe58f038f9b2ce80873',
                //         'content-type': 'application/json;charset=utf-8'
                //     },
                //     url,
                // };
                //
                // axios.get(url, { headers: options.headers})
                //     .then(response => {
                //         // If request is good...
                //         console.log(response.data);
                //     })
                //     .catch((error) => {
                //         console.log('error ' + error);
                //     });
                var data = null;

                var xhr = new XMLHttpRequest();
                xhr.withCredentials = true;

                xhr.addEventListener("readystatechange", function () {
                    if (this.readyState === 4) {
                        console.log(this.responseText);
                    }
                });

                xhr.open("GET", "https://od-api.oxforddictionaries.com:443/api/v2/entries/en-gb/hello");
                xhr.setRequestHeader("app_id", "dc4282d6");
                xhr.setRequestHeader("app_key", "281d675436705fe58f038f9b2ce80873");
                xhr.setRequestHeader("User-Agent", "PostmanRuntime/7.17.1");
                xhr.setRequestHeader("Accept", "*/*");
                xhr.setRequestHeader("Cache-Control", "no-cache");
                xhr.setRequestHeader("Postman-Token", "9bc84470-56f3-4786-ac48-6aa7bf4003f7,ddec4e2b-681e-427b-abe3-9de77cc9a2e7");
                xhr.setRequestHeader("Host", "od-api.oxforddictionaries.com:443");
                xhr.setRequestHeader("Accept-Encoding", "gzip, deflate");
                xhr.setRequestHeader("Connection", "keep-alive");
                xhr.setRequestHeader("cache-control", "no-cache");

                xhr.send(data);
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
