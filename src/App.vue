<template>
  <div id="app">
    <transition name="fade" mode="out-in">
    <router-view/>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
.fade-enter {
  opacity:0;
}
.fade-leave{
  opacity:1;
}
.fade-enter-active{
  transition:opacity .5s;
}
.fade-leave-active{
  opacity:0;
  transition:opacity .5s;
}

html,
body {
  margin: 0px;
  background-color: #f3f3f7;
  height: 100%;
  width: 100%;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  height: 100%;
  width: 100%;
  display: flex;
  flex-direction: column;
  /* justify-content: space-between; */
  /* align-items: center;  */
}

.container {
  flex: 1;
  width: 100%;
}

button,
div {
  outline: none;
}
button,
a {
  border: 0px;
  cursor: pointer;
}
input{
  padding-left: 15px;
}
::-moz-focus-inner {
  border: 0;
}
:focus {
  outline: none;
}

.click-icon {
  cursor: pointer;
  font-size: 16px;
}
.blue-icon {
  color: #4498c7;
  margin-right: 5px;
}
.red-icon {
  color: #ed5b53;
}
</style>
