<template>
  <b-container class="bv-example-row">
    <b-row>
      <Chat />
      <div class="w-100"></div>
    </b-row>
  </b-container>
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
