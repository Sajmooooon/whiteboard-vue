<template>
  <create-top-bar/>
  <div class="draggable">
    <create-sticker
        v-for="(sticker,i) in stickers"
        :key="i"
        :title="sticker.title"
        :text="sticker.text"
        :remove="sticker"
        @remove-sticker="removeSticker($event)"
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
        id: 0,
      }]
    }
  },
  components: {
    CreateSticker,
    CreateNavBar,
    CreateTopBar
  },
  methods:{
    getLastId(){
      if(!this.stickers.length){
        return 0
      }
      let index = this.stickers.length - 1
      let newId = this.stickers[index].id + 1
      return newId
    },

    addSticker(sticker){
      this.stickers.push({
        title: sticker.title,
        text: sticker.text,
        id: this.getLastId()
      })
    },

    removeSticker(obj){
      this.stickers = this.stickers.filter(item => item !== obj)
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