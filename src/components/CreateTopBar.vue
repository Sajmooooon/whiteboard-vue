<template>
  <header class="top-nav">
    <input value="1st whiteboard" :disabled="enabledDragging">
  </header>
</template>

<script>
export default {
  name: "CreateTopBar",
  props:{
    enabledDragging:{
      type: Boolean,
    }
  }
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

input{
  width: 250px;
  user-select:none;
  margin: auto 0 auto 25px;
  font-size: 1.4em;
  background: transparent;
  border: 0;

}
input:focus{
  outline: none;
  border-bottom: #888888 1px solid;
}

input:disabled {
  pointer-events:none;
}
</style>