<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <img :data-src="imageUrl" :src="imageUrl" class="commander-image" />
    <h3>Deck Name : {{ name }}</h3>
    <button v-on:click="rollDeck">Roll</button>
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
      name: "",
      imageUrl: "",
      cards: []
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
          var imageId = response.data.data.commander[0].identifiers.scryfallId;
          instance.cards = response.data.data.mainBoard.map(item => "https://api.scryfall.com/cards/" + item.identifiers.scryfallId + "?format=image")
          instance.imageUrl = "https://api.scryfall.com/cards/" + imageId + "?format=image";
        });
    },
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
