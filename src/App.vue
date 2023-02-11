<template>
  <div id="app">
    <ChatBoxe />
  </div>
</template>

<script>
import ChatBoxe from './components/ChatBoxe.vue'

export default {
  name: 'App',
  components: {
    ChatBoxe
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
}

#app {
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: url(./assets/bg.jpg) no-repeat;
  background-size: 100% 100%;
}

@media screen and (max-width: 600px) {
  #app {
    background: url(./assets/bg2.png) no-repeat;
  background-size: 100% 100%;

  }
}
</style>
