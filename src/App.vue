<template>
  <create-top-bar/>
  <div class="draggable">
    <create-sticker></create-sticker>
    <create-nav-bar/>
  </div>
</template>

<script>
import CreateSticker from "./components/CreateSticker.vue";
import CreateNavBar from "./components/CreateNavBar.vue";
import CreateTopBar from "./components/CreateTopBar.vue";

export default {
  name: "App",
  components: {
    CreateSticker,
    CreateNavBar,
    CreateTopBar
  },
}
</script>

<style>
.draggable{
  position: relative;
  width: 100%;
  height: calc(100vh - 50px );
}
</style>