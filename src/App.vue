<template>
  <create-top-bar
      :enabledDragging="enabledDragging"
  />
  <div class="draggable" id="test"
       @mouseleave="mouseLeave($event)"
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

      stickers: [{
        title: 'Tittle',
        text: 'text',
        id: 0,
        x: 40,
        y: 50,
        zIndex: 100,
        dragging: false,
        color: 'aqua'
      }]
    }
  },
  components: {
    CreateSticker,
    CreateNavBar,
    CreateTopBar
  },
  methods:{
    mouseLeave(e){
      if(this.enabledDragging){
        this.mouseOut = true
      }
    },

    mouseEnter(e){
      if(this.mouseOut){
        if(e.screenY< this.height/2){
          this.offsetY = this.topBarOffset
        }
        else{
          this.offsetY = this.topBarOffset + 250
        }

        if(e.screenX < this.width/2){
          this.offsetX = 0
        }
        else{
          this.offsetX = 250
        }
      }
    },

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
        dragging: false,
        color: sticker.color
      })
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
      return `translateX(${sticker.x}px) translateY(${sticker.y}px)`;
    },

    startDrag(event,sticker){
      if(this.enabledDragging){
        let rect = event.target.parentElement.getBoundingClientRect();
        this.cursorX =  (typeof event.clientX =="number") ? event.clientX : event.touches[0].clientX
        this.cursorY = (typeof event.clientY =="number") ? event.clientY : event.touches[0].clientY
        this.offsetX = this.cursorX - rect.left
        this.offsetY = this.cursorY - rect.top + this.topBarOffset

        let ind = this.stickers.findIndex(element => element === sticker)
        this.lastDragged = ind

        this.stickers[ind].dragging = true
      }

    },

    endDrag(event,sticker){
      if(this.enabledDragging){
        let ind = this.stickers.findIndex(element => element === sticker)
        this.stickers[ind].dragging = false
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
      this.cursorX = (typeof event.clientX =="number") ? event.clientX : event.touches[0].clientX
      this.stickers[this.lastDragged].x = this.cursorX - this.offsetX
    },

    moveY(event){
      this.cursorY = (typeof event.clientY =="number") ? event.clientY : event.touches[0].clientY
      this.stickers[this.lastDragged].y = this.cursorY - this.offsetY
    },

    changePos(event){
      if (this.lastDragged ===null || !this.stickers[this.lastDragged].dragging)
        return
      let sticker = document.querySelector('.dragging').getBoundingClientRect()

      if(this.cursorX > ((typeof event.clientX =="number") ? event.clientX : event.touches[0].clientX)){
        if(this.checkDragSmaller(sticker.left,0)){
          this.moveX(event)
        }
      }
      else{
        if(this.checkDragHigher(sticker.right,this.width)){
          this.moveX(event)
        }
      }
      if(this.cursorY > ((typeof event.clientY =="number") ? event.clientY : event.touches[0].clientY)){
        if(this.checkDragSmaller(sticker.top,this.topBarOffset)){
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
    this.height = draggable.clientHeight + this.topBarOffset

    let _this = this;
    window.addEventListener('mouseup', function(e) {
      if(_this.lastDragged!==null){
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