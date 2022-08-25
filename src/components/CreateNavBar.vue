<template>
  <nav class="navbar">
    <div class="navbar-body">
      <ul class="navbar-items">
        <li class="navbar-item center" @click="toggleDragging">
          <i class="navbar-item-icon center fa fa-hand-grab-o" :class="{active: enabledDragging}"></i>
        </li>
        <li class="navbar-item center" @click="createNewSticker">
          <i class="navbar-item-icon center fa fa-plus"></i>
        </li>
      </ul>
    </div>
  </nav>
</template>

<script>
export default {
  name: "CreateNavBar",
  data(){
    return{
      colors: ['purple', 'aqua', 'rubineRed', 'green','teal'],
      newSticker: {
        title: 'New Sticker Title',
        text: 'New Sticker Text',
        color: 'purple',
      },
    }
  },
  props:{
    enabledDragging:{
      type: Boolean,
    }
  },
  methods:{
    createNewSticker(){
      this.newSticker.color = this.generateColor()
      this.$emit('add-sticker',this.newSticker)
    },
    toggleDragging(){
      this.$emit('toggle-dragging')
    },
    generateColor(){
      return this.colors[Math.floor(Math.random()*(this.colors.length))]
    }
  }
}
</script>

<style scoped>
.navbar{
  width: 100%;
  bottom: 50px;
  position: absolute;
  z-index: 99999;

}

.navbar-body{
  border-radius: 5px;
  background-color: #ffffff;
  border: 2px solid #eee;
  width: min-content;
  margin: auto;
  align-items: center;
}

.navbar-items{
  display: flex;
  justify-content: center;
}

.center{
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.navbar-item{
  cursor: pointer;
  width: 50px;
  height: 50px;
}

.navbar-item-icon{
  font-size: 1.3rem;
  width: 40px;
  height: 40px;
  transition: 0.1s ease-out;
  font-weight: bold;
  border-radius: 10px;
}
.navbar-item-icon:hover{
  background-color: #fae8ff;
  color: #86198f;

}
.active{
  background-color: #fae8ff;
  color: #86198f;
}

.navbar-item:not(:last-child):after{
  content: '';
  position:absolute;
  /*left:50%;*/
  right: 0;
  top:15%;
  bottom:15%;
  border-left:1px solid black;
}

ul {
  padding-inline-start: 0;
  margin-block-start: 0;
  margin-block-end: 0;
  list-style-type: none;

}

li{
  text-decoration: none;
  position: relative;
}
</style>