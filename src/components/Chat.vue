<template>
  <div>
    <b-form inline v-if="!userRegister">
      <br/>
      <br/>
      <br/>
      <b-input-group class="mb-2 mr-sm-2 mb-sm-0">
        <b-input id="inline-form-input-username" v-model="user" placeholder="Username"></b-input>
      </b-input-group>
      <b-button variant="primary" type="button" v-on:click="crearUsuario">Crear</b-button>
    </b-form>
    <div class="card mt-3" v-if="userRegister">
      <div class="card-body">
        <div class="messages" v-for="message in messages" :key="message">
          {{ message.author }}:
          <b-list-group-item href="#" variant="secondary">{{ message.message }}</b-list-group-item>
        </div>
      </div>
      <div class="card-footer">
        <form inline @submit.prevent="sendMessage">
          <div class="gorm-group pb-2">
            <input type="text" v-model="message" class="form-control">
          </div>
          <button type="submit" class="btn btn-success">Send</button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import io from 'socket.io-client'
var socket = null
export default {
  name: 'Chat',
  props: {
    msg: String
  },
  data () {
    return {
      user: '',
      userRegister: false,
      message: '',
      messages: []
    }
  },
  created: function () {
    socket = io('http://localhost:5000/namespace1')
    socket.on('message', (data) => {
      let msgs = this.messages
      msgs.push({
        message: data.message,
        author: data.author
      })
      this.messages = msgs
    })
  },
  methods: {
    sendMessage (e) {
      e.preventDefault()
      /** namespace */
      socket.emit('message', {
        author: this.user,
        message: this.message
      })
      this.message = ''
    },
    crearUsuario (e) {
      e.preventDefault()
      console.log('preventdefault', this.user)
      if (this.user !== '') {
        this.userRegister = true
      }
    }
  }
}
</script>
