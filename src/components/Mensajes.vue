<template>
  <b-list-group>
    <b-list-group-item v-for="message in messages" :key="message">{{ message }}</b-list-group-item>
  </b-list-group>
</template>

<script>
import io from 'socket.io-client'

export default {
  name: 'Mensajes',
  props: {
    msg: String
  },
  data () {
    return {
      user: '',
      message: '',
      messages: [],
      socket: io('localhost:5000/msg')
    }
  },
  methods: {
    sendMessage (e) {
      e.preventDefault()
    }
  }
}
</script>
