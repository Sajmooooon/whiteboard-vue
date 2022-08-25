<template>
  <div class="sticker" :class="{dragging: sticker.dragging, cursor: enabledDragging}, sticker.color">
    <div class="sticker-out">
      <i class="sticker-del fa fa-close" :class="{active: !enabledDragging}" @click="removeSticker"></i>
      <div class="sticker-inside">
          <span class="noselect sticker-title textarea" spellcheck="false" :contenteditable="!sticker.dragging" @keyup="updateStickerTitle">{{title}}</span>
          <span class="noselect sticker-text textarea" spellcheck="false" :contenteditable="!sticker.dragging" @keyup="updateStickerText">{{text}}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CreateSticker",
  props: {
    sticker:{
      type: Object,
    },
    enabledDragging:{
      type: Boolean,
    }
  },
  data(){
    return{
      title: this.sticker.title,
      text: this.sticker.text
    }
  },
  methods: {
    removeSticker(){
      this.$emit('remove-sticker',this.sticker)
    },
    updateStickerText(e){
      this.sticker.text = e.target.innerText
      this.$emit('update-sticker',this.sticker)
    },
    updateStickerTitle(e){
      this.sticker.title = e.target.innerText
      this.$emit('update-sticker',this.sticker)
    }
  }
}
</script>

<style scoped>
.purple{
  background-color: #BB29BB;
}
.aqua{
  background-color: #00FFFFFF;
}
.rubineRed{
  background-color: #CE0058;
}

.green{
  background-color: #00AB84;
}

.teal{
  background-color: #03C0C1;
}

.textarea {
  display: block;
  width: 100%;
  overflow: hidden;
  resize: none;
  min-height: 35px;
  line-height: 20px;
  outline: none;

}

.textarea:focus{
  border-bottom: #888888 1px solid;
}

.sticker-title{
  font-weight: bold;
  font-size: 22px;
  margin-bottom: 10px;
}

.sticker-text{
  font-size: 17px;
}

.active{
  display: none;
}

.noselect {
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;

}

.sticker{
  opacity: 0.9;
  position: absolute;
  overflow: hidden;
  word-break: break-all;
  width: 250px;
  height: 250px;
  min-width: 100px;
  min-height: 100px;
  max-width: 600px;
  max-height: 600px;
  /*background-color: aqua;*/
  box-shadow: 10px 10px 5px -5px rgba(212,212,212,0.75);
  -webkit-box-shadow: 10px 10px 5px -5px rgba(212,212,212,0.75);
  -moz-box-shadow: 10px 10px 5px -5px rgba(212,212,212,0.75);

}

.sticker-out:hover .active{
  display: block;
}

.cursor:hover{
  cursor: grab;
}

.sticker-del{
  display: none;
  z-index: 1;
  cursor: pointer;
  color: #000000;
  font-size: 2.3rem;
  position: absolute;
  top: 0;
  right: 5px;
}

.sticker-out{
  position: relative;
  width: 100%;
  height: 100%;
}

.sticker-inside{
  position: relative;
  padding: 7%;
  height: 86%;
  overflow-y: auto;

}

.sticker-inside::-webkit-scrollbar {
  width: 2px;
}

.sticker-inside::-webkit-scrollbar-thumb {
  background: #888;
}

.sticker-head{
  font-weight: bold;
}

.sticker-head h3{
  margin-top: 5px;
}

.sticker-body p{
  margin-bottom: 0;
}


</style>