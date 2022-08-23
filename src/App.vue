<template>
  <create-top-bar/>
  <div class="draggable" id="test"
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
      width: 0,
      height: 0,
      enabledDragging: false,
      x: null,
      y: null,
      lastDragged: null,
      offsetX: 0,
      offsetY: 0,

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
        let rect = event.target.getBoundingClientRect();
        this.offsetX = event.clientX - rect.left
        this.offsetY = event.clientY - rect.top + 50

        let ind = this.stickers.findIndex(element => element === sticker)
        this.lastDragged = ind
        this.x =  event.clientX
        this.y = event.clientY
        this.stickers[ind].dragging = true
      }

    },

    checkDragSmaller(site,len){
      if(site>=len){
        return true
      }
    },

    checkDragHigher(site,len){
      if(site<=len){
        return true
      }
    },

    moveX(event){
      this.stickers[this.lastDragged].x = event.clientX - this.offsetX
      this.x = event.clientX
    },
    moveY(event){
      this.stickers[this.lastDragged].y = event.clientY - this.offsetY
      this.y = event.clientY
    },

    changePos(event){
      if (this.lastDragged ===null || !this.stickers[this.lastDragged].dragging)
        return
      let sticker = document.querySelector('.dragging').getBoundingClientRect()

      if(this.x > event.clientX){
        if(this.checkDragSmaller(sticker.left,0)){
          this.moveX(event)
        }

      }
      else{
        if(this.checkDragHigher(sticker.right,this.width)){
          this.moveX(event)
        }
      }

      if(this.y > event.clientY){
        if(this.checkDragSmaller(sticker.top,50)){
          this.moveY(event)
        }
      }
      else{
        if(this.checkDragHigher(sticker.bottom,this.height)){
          this.moveY(event)
        }
      }
    },

    toggleDragging(){
      this.enabledDragging = !this.enabledDragging
    }

  },

  mounted() {
    let draggable = document.querySelector('.draggable')
    this.width = draggable.clientWidth
    this.height = draggable.clientHeight + 50

    let _this = this;
    window.addEventListener('mouseup', function(e) {
      if(_this.lastDragged!==null)
        _this.stickers[_this.lastDragged].dragging =false
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