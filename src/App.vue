<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">

      </div>

      <v-spacer></v-spacer>
    </v-app-bar>

    <v-content>
      <v-container fluid>
        <v-row>
          <ul>
            <li v-for="(message, index) in messages.slice(0, next)" v-bind:key="index" :class="message.owner">
              
              <vue-typed-js :fadeOut="true" :strings="[message.text]">
                <h1 v-if="message.owner=='him'" class="typing"></h1>
                <h1 class="not-typing" v-else>{{message.text}}</h1>
              </vue-typed-js>
            </li>
          </ul>
        </v-row>
        <v-row :style="{position: 'absolute', bottom: 0, left: 0, right: 0, padding: '10px', zIndex: 100, background: '#ffffff'}">
          <v-col cols="12" md="12">
            <v-textarea
                    filled
                    name="input-7-1"
                    label="Type your message"
                    v-model="input"
            ></v-textarea>
          </v-col>
          <v-col cols="12" md="12">
            <div class="my-2">
              <v-btn v-if="newChat" @click="send" x-large :style="{position: 'absolute', bottom: 0, right: 0, background: '#56c8d8'}" color="primary" dark>Let's chat</v-btn>
              <v-btn v-else-if="!endChat" @click="send" x-large :style="{position: 'absolute', bottom: 0, right: 0, background: '#56c8d8'}" color="primary" dark>Send Message</v-btn>
              <v-btn v-else x-large :style="{position: 'absolute', bottom: 0, right: 0, background: '#56c8d8'}" disabled color="primary" dark>Send Message</v-btn>
            </div>
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>

import axios from 'axios';
import {VueTypedJs} from 'vue-typed-js';


export default {
  name: 'App',

  components: {
    VueTypedJs
  },

  data: () => ({
    name: '',
    age: 0,
    location: '',
    feeling: '',
    hobby: '',
    next: 0,
    input: '',
    toChat: [],
    newChat: true,
    endChat: false,
    messages: [],
  }),
  mounted() {
    this.getMessage();
  },
  methods: {
    getMessage(){//function handles getting messages with axios
      axios.get("messages.json").then(response => {
      this.messages = response.data;
    })
    },
    myMessage(){//function handles update all message with users message
      if(this.input.length > 0){
            this.name = this.input;
            this.messages.splice(this.next, 0,{
              text: this.input,
              owner: 'me'
            })
            this.storeResponse();
            this.input = "";
        }
    },
    storeResponse() {//function stores a user answer to previous specific question asked by bot and utilize the answer for its response
      if (this.messages[this.next-1].ask === 'name') { //store user name
        this.name = this.input
        this.messages[this.next+1].text = this.messages[this.next+1].text +" "+this.name; //lets tell the bot the users name
      }else if (this.messages[this.next-1].ask === 'location') { //store user location
        this.location = this.input;
        this.messages[this.next+1].text =  this.location +", "+ this.messages[this.next+1].text//lets tell the bot the users name
      }else if (this.messages[this.next-1].ask === 'feeling'){ //store user feeling
        this.feeling = this.input;
      }else if (this.messages[this.next-1].ask === 'hobby'){ //store user hobby
        this.hobby = this.input;
      }else if (this.messages[this.next-1].ask === 'age'){ //store user age
        this.age = parseInt(this.input);
      }
    },
    send() {//function handles sending all messages
    
      if(this.newChat){//catch my first message to the bot
        this.newChat = false;
        this.myMessage();
      }

      let active = true;
      
      while(active) {
        this.scrollToBottom();
        this.myMessage();

        if (typeof this.messages[this.next].ask === 'undefined') {
          if(this.messages[this.next] == this.messages[this.messages.length -1]){//check if this is the last bot message then disable send message button
            this.endChat = true;
          }
        } else {
          active = false;
        }
        this.next += 1;
      }
      
    },
    scrollToBottom(){ //function handles scroll to bottom feature  	
      var container = this.$el.querySelector("ul");
      container.scrollTop = container.scrollHeight;
    },
  }
};
</script>

<style>
  ul{
    list-style: none;
    margin: 0;
    padding: 0;
    position: absolute;
    left: 0;
    right: 0;
    overflow-y: scroll;
    height:600px;
    z-index: 0;
    padding-bottom: 100px;
  }

  ul li{
    display:inline-block;
    clear: both;
    padding: 20px;
    border-radius: 30px;
    margin-bottom: 2px;
    font-family: Helvetica, Arial, sans-serif;
  }

  .typing, .not-typing{
    font-size: 14px;
  }

  .him{
    background: #eee;
    float: left;
  }

  .me{
    background: #0084ff;
    float: right;
  }

  .me{
    float: right;
    background: #0084ff;
    color: #fff;
  }

  .him + .me{
    border-bottom-right-radius: 5px;
  }

  .me + .me{
    border-top-right-radius: 5px;
    border-bottom-right-radius: 5px;
  }

  .me:last-of-type {
    border-bottom-right-radius: 30px;
  }
</style>
