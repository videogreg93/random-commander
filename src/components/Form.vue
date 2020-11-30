<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <img :data-src="imageUrl" :src="imageUrl" class="commander-image" />
    <h3>Deck Name : {{ name }}</h3>
    <button v-on:click="rollDeck">Roll</button>
    <button v-if="name" v-clipboard="copyDecklist">Copy decklist to clipboard</button>
    <div class="cards-container">
      <div v-for="card in cards" :key="card">
      <img :data-src=card :src=card class="card" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Decklist from "../assets/deckList.json";

export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      name: null, // Commander deck name
      imageUrl: "", // Commander image url
      cards: [], //Card Images
      decklist: null // Cardlist for importing
    };
  },
  methods: {
    rollDeck: function () {
      const random = Math.floor(Math.random() * this.commanderDecks.length);
      var deck = this.commanderDecks[random];
      this.name = this.commanderDecks[random].name;
      var instance = this;
      axios
        .get("https://mtgjson.com/api/v5/decks/" + deck.fileName + ".json")
        .then((response) => {
          console.log(response);
          var commander = response.data.data.commander[0];
          var imageId = commander.identifiers.scryfallId;
          instance.cards = response.data.data.mainBoard.map(item => "https://api.scryfall.com/cards/" + item.identifiers.scryfallId + "?format=image")
          instance.imageUrl = "https://api.scryfall.com/cards/" + imageId + "?format=image";
          instance.decklist = "1 " + commander.name + "\n";
          response.data.data.mainBoard.forEach((item) => {
            instance.decklist += item.count + " " + item.name + "\n";
          });
          //instance.decklist = response.data.data.mainBoard.map(item => item.count + " " + item.name);
        });
    },
    copyDecklist: function () {
      return this.decklist;
    }
  },
  mounted() {
    this.commanderDecks = Decklist["data"].filter((deck) =>
      deck.type.includes("Commander")
    );

    console.log(this.commanderDecks);
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.commander-image {
  width: 300px;
}
.cards-container {
  display: flex; /* or inline-flex */
  flex-flow: row wrap;
}
.card {
  width: 200px;
  margin: 10px;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
