<template>
  <div class="header">
    <ul>
      <li><img src="../assets/desk-lamp.svg"/></li>
      <li><a href="#">HOME</a></li>
      <li><a href="#">COMMUNITY</a></li>
      <li><a href="#">FAQ</a></li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'Header',
  props: {
    msg: String
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
