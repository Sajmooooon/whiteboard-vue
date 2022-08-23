<template>
  <create-top-bar/>
  <div class="draggable"
       @mousemove="changePos($event)">
    <create-sticker
        v-for="(sticker,i) in stickers"
        :key="i"
        :sticker="sticker"
        :enabledDragging="enabledDragging"
        @remove-sticker="removeSticker($event)"
        @mousedown="startDrag($event,sticker)"

        :style="{transform: getTransform(sticker),
                       zIndex: sticker.zIndex,}"

    />
    <create-nav-bar
        :enabledDragging="enabledDragging"
        @add-sticker="addSticker($event)"
        @toggle-dragging="toggleDragging"
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
      enabledDragging: false,
      lastDragged: null,
      stickers: [{
        title: 'Tittle',
        text: 'asdsa',
        id: 0,
        x: 40,
        y: 50,
        zIndex: 100,
        dragging: false,
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
      return this.stickers[index].id + 1
    },

    addSticker(sticker){
      let newId = this.getLastId();
      this.stickers.push({
        title: sticker.title,
        text: sticker.text,
        id: newId,
        x: 50,
        y: 40,
        zIndex: 100 + newId,
        dragging: false
      })
    },

    removeSticker(obj){
      this.stickers = this.stickers.filter(item => item !== obj)
      this.lastDragged = null
    },

    getTransform(sticker){
      return `translateX(${sticker.x}px) translateY(${sticker.y}px)`;
    },

    startDrag(event,sticker){
      if(this.enabledDragging){
        let ind = this.stickers.findIndex(element => element === sticker)
        this.lastDragged = ind
        this.stickers[ind].dragging =true;
      }

    },

    changePos(event){
      if (this.lastDragged ===null || !this.stickers[this.lastDragged].dragging)
        return
      this.stickers[this.lastDragged].x = event.clientX-(250/2);
      this.stickers[this.lastDragged].y = event.clientY-(250/2);
    },

    toggleDragging(){
      this.enabledDragging = !this.enabledDragging
    }

  },

  mounted() {
    let _this = this;
    window.addEventListener('mouseup', function(e) {
      if(_this.lastDragged!==null)
        _this.stickers[_this.lastDragged].dragging =false;
    }, );


  }
}
</script>

<style>
.draggable{
  overflow: hidden;
  position: relative;
  width: 100%;
  height: calc(100vh - 50px );
}
</style>