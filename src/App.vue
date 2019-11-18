<template>
  <div id="app">
    <b-container class="bv-example-row">
      <b-row>
        <Chat />
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Chat from '@/components/Chat.vue'

export default {
  name: 'home',
  components: {
    Chat
  }
}
</script>

<style lang="stylus">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 60px
</style>
