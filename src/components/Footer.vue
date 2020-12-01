<template>
  <div class="container">
    <footer>The information presented on this site about Magic: The Gathering, both literal and graphical, is copyrighted by Wizards of the Coast.
This website is not produced, endorsed, supported, or affiliated with Wizards of the Coast.</footer>
  </div>
</template>

<script>

export default {
  name: "Footer",
  mounted() {},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  display: flex; /* or inline-flex */
  flex-flow: column wrap;
  align-items: center;
  justify-content: center;
}
footer {
  font-size: small;
  width: 50%;
  text-align: center;
  flex: 1;
  margin-top: 300px;
}
</style>
