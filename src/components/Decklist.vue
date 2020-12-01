<template>
  <div>
    <auto-card-list
      name="SAM BLACK'S LANTERN CONTROL"
      src="../assets/test.dec"
    ></auto-card-list>
  </div>
</template>

<script>
import axios from "axios";
import Decklist from "../assets/deckList.json";

export default {
  name: "Decklist",
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
  mounted() {},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
