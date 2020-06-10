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
            <li v-for="(message, index) in messages.slice(0, next)" v-bind:key="index" :class="message.owner">{{message.text}}</li>
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
              <v-btn @click="send" x-large :style="{position: 'absolute', bottom: 0, right: 0, background: '#56c8d8'}" color="primary" dark>Send Message</v-btn>
            </div>
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  components: {

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
    messages: [
      {
        text: "Hi, I'm Peter!",
        owner: 'him'
      },
      {
        text: "What's your name?",
        ask: "name",
        owner: 'him'
      },
      {
        text: "Nice to meet you!",
        owner: 'him'
      },
      {
        text: "How was your day?",
        ask: "feeling",
        owner: 'him'
      },
      {
        text: "Where're you from?",
        ask: "location",
        owner: 'him'
      },
      {
        text: "Nice!",
        owner: 'him'
      },
      {
        text: "How old are you?",
        ask: "age",
        owner: 'him'
      },
      {
        text: "What's your favorite hobby?",
        ask: "hobby",
        owner: 'him'
      },
      {
        text: "Wow, cool",
      }
    ]
  }),
  methods: {
    send() {
      let active = true
      while(active) {

        if (typeof this.messages[this.next].ask === 'undefined') {
          this.next += 1;
        } else {
          this.next += 1;
          if (this.messages[this.next].ask === 'name') {
            this.name = this.input
            this.messages.splice(this.next, 0,{
              text: this.input,
              owner: 'me'
            })
          }
          active = false;
        }
      }
    }
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

  .him{
    background: #eee;
    float: left;
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
