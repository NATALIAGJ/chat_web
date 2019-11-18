<template>
  <b-container>
    <b-form inline v-if="!userRegister">
      <b-input-group class="mb-2 mr-sm-2 mb-sm-0">
        <b-input id="inline-form-input-username" v-model="user" placeholder="Username"></b-input>
      </b-input-group>
      <b-button variant="primary" type="button" v-on:click="crearUsuario">Crear</b-button>
    </b-form>
    <div v-if="userRegister">
      <b-card no-body>
        <b-card-body
          id="nav-scroller"
          ref="content"
          style="position:relative; height:300px; overflow-y:scroll;"
        >
          <div class="card-body">
            <strong>{{ user }}</strong>
            <br/>
            <br/>
            <br/>
            <div class="messages" v-for="message in messages" :key="message">
              <div v-if="message.author === user">
                <b-row>
                  <b-col></b-col>
                  <b-col variant="success">
                    <b-list-group-item variant="success">{{ message.message }}</b-list-group-item>
                    <span style="font-size=20px;">{{ formatDate(message.date) }}</span>
                    <br/>
                  </b-col>
                </b-row>
              </div>
              <div v-else>
                <b-row>
                  <b-col variant="success">
                    <b-list-group-item variant="warning">{{ message.message }}</b-list-group-item>
                    {{ formatDate(message.date) }}
                    <br/>
                  </b-col>
                  <b-col></b-col>
                </b-row>
              </div>
              <!--<strong>{{ message.author }}</strong>: {{ message.message }}-->
            </div>
          </div>
        </b-card-body>
      </b-card>
    </div>
    <div class="card mt-3" v-if="userRegister">
      <form inline @submit.prevent="sendMessage">
        <div class="gorm-group pb-2">
          <input type="text" v-model="message" class="form-control">
        </div>
        <button type="submit" class="btn btn-success">Send</button>
      </form>
    </div>
  </b-container>
</template>

<script>
import io from 'socket.io-client'
import moment from 'moment'

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
      color: '',
      messages: []
    }
  },
  created: function () {
    socket = io('http://localhost:5000/namespace1')
    socket.on('message', (data) => {
      let msgs = this.messages
      msgs.push({
        message: data.message,
        author: data.author,
        date: moment()
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
      if (this.user !== '') {
        this.userRegister = true
      }
    },
    formatDate (date) {
      return moment(date).format('h:mm')
    }
  }
}
</script>
