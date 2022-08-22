<template>
  <header class="top-nav">
    <h1>Jane's 1st whiteboard</h1>
  </header>
</template>

<script>
export default {
  name: "CreateTopBar"
}
</script>

<style scoped>
.top-nav{
  color: #000000;
  z-index: 99999;
  top: 0;
  width: 100%;
  height: 50px;
  background-color: #ffffff;
  box-shadow: -5px -5px 10px  #000000;
  -webkit-box-shadow: -5px -5px 10px  #000000;
  -moz-box-shadow: -5px -5px 10px  #000000;
  display: flex;
}

.top-nav h1{
  margin: auto 0 auto 25px;
  font-size: 1.7em;
}
</style>