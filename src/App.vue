<template>
  <create-top-bar
      :enabledDragging="enabledDragging"
  />
  <div class="draggable" id="test"
       @mouseleave="mouseLeave"
       @mouseenter="mouseEnter($event)"
       @mousemove="changePos($event)"
       @touchmove="changePos($event)"
  >
    <create-sticker
        v-for="sticker in stickers"
        :key="sticker.id"
        :sticker="sticker"
        :enabledDragging="enabledDragging"
        @remove-sticker="removeSticker($event)"
        @update-sticker="updateSticker($event)"
        @mousedown="startDrag($event,sticker)"
        @touchstart="startDrag($event,sticker)"
        @touchend="endDrag($event,sticker)"
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
      mouseOut: false,
      topBarOffset: 50,
      width: 0,
      height: 0,
      enabledDragging: false,
      cursorX: null,
      cursory: null,
      lastDragged: null,
      offsetX: 0,
      offsetY: 0,
      colors: ['purple', 'aqua', 'rubineRed', 'green', 'teal'],

      drag: {
        type: '',
        x: 50,
        y: 40,
        id: 0,
        zIndex: 100,
        dragging: false,
      },

      sticker: {
        title: 'New Sticker Title',
        text: 'New Sticker Text',
        color: ''
      },

      stickers: [{
        title: 'Title',
        text: 'text',
        color: 'aqua',
        type: 'sticker',
        id: 0,
        x: 40,
        y: 50,
        zIndex: 100,
        dragging: false
      }]
    }
  },
  components: {
    CreateSticker,
    CreateNavBar,
    CreateTopBar
  },
  methods:{

    mouseLeave(){
      if(this.enabledDragging){
        this.mouseOut = true
      }
    },

    getNewOffset(cursor, half, newOffset1, newOffset2){
      if(cursor < half){
        return newOffset1
      }
      else{
        return newOffset2
      }
    },

    mouseEnter(e){
      if(this.mouseOut){
        this.offsetY = this.getNewOffset(e.screenY, this.height/2, this.topBarOffset, this.topBarOffset+248)
        this.offsetX = this.getNewOffset(e.screenX, this.width/2, 2, 248)
      }
    },

    getLastId(){
      if(!this.stickers.length){
        return 0
      }
      let index = this.stickers.length - 1
      return this.stickers[index].id + 1
    },

    generateColor(){
      return this.colors[Math.floor(Math.random()*(this.colors.length))]
    },

    addSticker(type){
      let newId = this.getLastId()
      if(type === "sticker"){
        let newSticker = Object.assign({},this.sticker, this.drag)
        newSticker.id = newId
        newSticker.zIndex = 100 + newId
        newSticker.type = type
        newSticker.color = this.generateColor()
        this.stickers.push(newSticker)
      }
    },

    removeSticker(obj){
      this.stickers = this.stickers.filter(item => item !== obj)
      this.lastDragged = null
    },

    updateSticker(obj){
      const ind = this.stickers.findIndex(item=>{
        return item.id === obj.id
      })
      this.stickers[ind].text = obj.text
      this.stickers[ind].title = obj.title
    },

    getTransform(sticker){
      return `translateX(${sticker.x}px) translateY(${sticker.y}px)`
    },

    startDrag(e,sticker){
      if(this.enabledDragging){
        let rect = e.target.parentElement.getBoundingClientRect()
        this.cursorX =  (typeof e.clientX =="number") ? e.clientX : e.touches[0].clientX
        this.cursorY = (typeof e.clientY =="number") ? e.clientY : e.touches[0].clientY
        this.offsetX = this.cursorX - rect.left
        this.offsetY = this.cursorY - rect.top + this.topBarOffset

        let ind = this.stickers.findIndex(element => element === sticker)
        this.lastDragged = ind

        this.stickers[ind].dragging = true
      }

    },

    endDrag(e,sticker){
      if(this.enabledDragging){
        let ind = this.stickers.findIndex(element => element === sticker)
        this.stickers[ind].dragging = false
      }
    },

    checkDragSmaller(site,len){
      if(site >= len){
        return true
      }
    },

    checkDragHigher(site,len){
      if(site <= len){
        return true
      }
    },

    moveX(e){
      this.cursorX = (typeof e.clientX === "number") ? e.clientX : e.touches[0].clientX
      this.stickers[this.lastDragged].x = this.cursorX - this.offsetX
    },

    moveY(e){
      this.cursorY = (typeof e.clientY === "number") ? e.clientY : e.touches[0].clientY
      this.stickers[this.lastDragged].y = this.cursorY - this.offsetY
    },

    changePos(e){
      if (this.lastDragged ===null || !this.stickers[this.lastDragged].dragging)
        return
      let sticker = document.querySelector('.dragging').getBoundingClientRect()

      if(this.cursorX > ((typeof e.clientX =="number") ? e.clientX : e.touches[0].clientX)){
        if(this.checkDragSmaller(sticker.left,0)){
          this.moveX(e)
        }
      }
      else{
        if(this.checkDragHigher(sticker.right,this.width)){
          this.moveX(e)
        }
      }
      if(this.cursorY > ((typeof e.clientY =="number") ? e.clientY : e.touches[0].clientY)){
        if(this.checkDragSmaller(sticker.top,this.topBarOffset)){
          this.moveY(e)
        }
      }
      else{
        if(this.checkDragHigher(sticker.bottom,this.height)){
          this.moveY(e)
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
    this.height = draggable.clientHeight + this.topBarOffset

    let _this = this;
    window.addEventListener('mouseup', function() {
      if(_this.lastDragged !== null){
        if(_this.stickers[_this.lastDragged].dragging){
          _this.stickers[_this.lastDragged].dragging = false
        }
      }
    }, )

  }
}
</script>

<style scoped>
.draggable{
  overflow: hidden;
  position: relative;
  width: 100%;
  height: calc(100vh - 50px );
}
</style>