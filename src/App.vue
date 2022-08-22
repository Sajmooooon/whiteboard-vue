<template>
  <create-top-bar/>
  <div class="draggable">
    <create-sticker
        v-for="(sticker,i) in stickers"
        :key="i"
        :title="sticker.title"
        :text="sticker.text"
    />
    <create-nav-bar
        @add-sticker="addSticker($event)"
    />
  </div>
</template>

<script>
import CreateSticker from "./components/CreateSticker.vue";
import CreateNavBar from "./components/CreateNavBar.vue";
import CreateTopBar from "./components/CreateTopBar.vue";

export default {
  name: "App",
  data(){
    return{
      stickers: [{
        title: 'Tittle',
        text: 'asdsa',
      }]
    }
  },
  components: {
    CreateSticker,
    CreateNavBar,
    CreateTopBar
  },
  methods:{
    addSticker(obj){
      this.stickers.push(obj)
    }
  }
}
</script>

<style>
.draggable{
  position: relative;
  width: 100%;
  height: calc(100vh - 50px );
}
</style>