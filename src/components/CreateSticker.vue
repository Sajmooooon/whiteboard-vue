<template>
  <div class="sticker">
    <div class="sticker-out">
      <i class="sticker-del fa fa-close" @click="removeSticker"></i>
      <div class="sticker-inside">
        <div class="sticker-head">
          <h3 class="noselect">{{sticker.title}}</h3>
        </div>
        <div class="sticker-body">
          <p class="noselect">{{sticker.text}}</p>
        </div>
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
    }
  },
  methods: {
    removeSticker(){
      this.$emit('remove-sticker',this.sticker)
    },
  }
}
</script>

<style scoped>
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
  background-color: aqua;
  box-shadow: 10px 10px 5px -5px rgba(212,212,212,0.75);
  -webkit-box-shadow: 10px 10px 5px -5px rgba(212,212,212,0.75);
  -moz-box-shadow: 10px 10px 5px -5px rgba(212,212,212,0.75);
  cursor: grab;

}

.sticker-out:hover .sticker-del{
  display: block;
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