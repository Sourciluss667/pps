<template>
  <div class="home">
    <h1>Choose your game ?</h1>

    <a href="/singleplayer" class="choose">Singleplayer</a>
    <br />
    <a href class="choose">Multiplayer</a>
  </div>
</template>

<script>
// @ is an alias to /src
export default {
  name: 'Home',
  components: {}
}
</script>

<style scoped>
h1 {
  color: white;
}

.choose {
  display: inline-block;
  padding: 0.35em 1.2em;
  border: 0.1em solid #ffffff;
  margin: 0 0.3em 0.3em 0;
  border-radius: 0.12em;
  box-sizing: border-box;
  text-decoration: none;
  font-family: "Roboto", sans-serif;
  font-weight: 300;
  color: #ffffff;
  text-align: center;
  transition: all 0.2s;
}
.choose:hover {
  color: #000000;
  background-color: #ffffff;
}
@media all and (max-width: 30em) {
  .choose {
    display: block;
    margin: 0.4em auto;
  }
}
</style>
