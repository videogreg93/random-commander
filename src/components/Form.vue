<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <img :data-src="imageUrl" :src="imageUrl" class="commander-image" />
    <h3 v-if="name">Deck Name : {{ name }}</h3>
    <button v-on:click="rollDeck" :disabled="enabledTypes.length === 0">Roll</button>
    <button v-if="name" v-clipboard="copyDecklist">
      Copy decklist to clipboard
    </button>
    <h3>Filters</h3>
    <div class="type-container">
      <div v-for="type in allTypes" :key="type" class="type">
        <input
          type="checkbox"
          :id="type"
          :name="type"
          :value="type"
          v-model="enabledTypes"
        />
        <label :for="type">{{ type }}</label>
      </div>
    </div>

    <button v-on:click="selectAll">Select All</button>
    <button v-on:click="selectNone">Select None</button>
    <h3 v-if="decklist">Cards</h3>
    <div class="cards-container">
      <div v-for="card in cards" :key="card">
        <img :data-src="card" :src="card" class="card" />
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
      decklist: null, // Cardlist for importing,
      allTypes: [],
      enabledTypes: [],
      commanderDecks: null,
    };
  },
  methods: {
    rollDeck: function () {
      var actualDecks = this.commanderDecks.filter((item) =>
        this.enabledTypes.includes(item.code)
      );
      const random = Math.floor(Math.random() * actualDecks.length);
      var deck = actualDecks[random];
      this.name = actualDecks[random].name;
      var instance = this;
      axios
        .get("https://mtgjson.com/api/v5/decks/" + deck.fileName + ".json")
        .then((response) => {
          console.log(response);
          var commander = response.data.data.commander[0];
          var imageId = commander.identifiers.scryfallId;
          instance.cards = response.data.data.mainBoard.map(
            (item) =>
              "https://api.scryfall.com/cards/" +
              item.identifiers.scryfallId +
              "?format=image"
          );
          instance.imageUrl =
            "https://api.scryfall.com/cards/" + imageId + "?format=image";
          instance.decklist = "1 " + commander.name + "\n";
          response.data.data.mainBoard.forEach((item) => {
            instance.decklist += item.count + " " + item.name + "\n";
          });
        });
    },
    copyDecklist: function () {
      return this.decklist;
    },
    selectAll: function () {
      this.enabledTypes.length = 0;
      this.allTypes.forEach((element) => {
        this.enabledTypes.push(element);
      });
    },
    selectNone: function () {
      this.enabledTypes = []; // Removes all items from the array
    },
  },
  mounted() {
    this.commanderDecks = Decklist["data"].filter((deck) =>
      deck.type.includes("Commander")
    );
    this.allTypes = new Set(this.commanderDecks.map((item) => item.code));
    this.allTypes.forEach((element) => {
      this.enabledTypes.push(element);
    });
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
button {
  margin: 5px;
}
.type-container {
  display: flex; /* or inline-flex */
  flex-flow: row wrap;
  align-items: center;
  justify-content: center;
}
.type {
  margin: 10px;
}
.commander-image {
  width: 300px;
}
.cards-container {
  display: flex; /* or inline-flex */
  flex-flow: row wrap;
  align-items: center;
  justify-content: center;
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
