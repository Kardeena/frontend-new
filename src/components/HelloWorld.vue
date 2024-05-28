<template>
  <div class="hello">
    <h1>Kardenas App</h1>
    <h2>What I'm doing</h2>	
    <h3>Eine Kalender-App</h3>
    <input type="text" v-model="inputValue" placeholder="Geben Sie etwas ein">
    <p v-if="inputValue">Sie haben eingegeben: {{ inputValue }}</p>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      inputValue: ''
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
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
