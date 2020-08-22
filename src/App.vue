<template>
  <v-app>
    <app-bar/>

    <v-main>
      <v-container class="wrapper" fluid>
        <chat-content :items="thread" />

        <chat-input
          @input="send"
          :disabled="isEnd"
          :is-first-message="isFirstMessage"
        />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import axios from 'axios';
import ChatContent from './components/ChatContent';
import ChatInput from './components/ChatInput';
import AppBar from './components/AppBar';

let _timer = null;
const BOT_MESSAGE_DELAY = 1000;

export default {
  name: 'App',
  components: {
    ChatContent,
    ChatInput,
    AppBar
  },
  async created() {
    try {
      this.messages = await axios.get('http://localhost:3000/messages').then((res) => {
        return res.data;
      });
    } catch (e) {
      console.error(e);

      this.messages = [];
    }
  },
  data: () => ({
    userInfo: {
      name: '',
      age: 0,
      location: '',
      feeling: '',
      hobby: '',
    },
    next: 0,
    thread: [],
    messages: []
  }),
  computed: {
    isFirstMessage() {
      return !this.thread.length;
    },
    isEnd() {
      return this.thread[this.thread.length - 1] === this.messages[this.messages.length - 1];
    },
    currentMessage() {
      return this.messages[this.next - 1];
    },
    isNextExists() {
      return this.messages[this.next];
    },
    isNextNotQuestion() {
      return this.isNextExists && !this.messages[this.next].ask;
    }
  },
  methods: {
    send(input) {
      this.pushMyMessage(input);

      if (this.currentMessage && this.currentMessage.ask) {
        this.setUserInfo(this.currentMessage.ask, input);
      }

      while(this.isNextNotQuestion) {
        this.pushBotAndNext();
      }

      if (this.isNextExists) {
        clearTimeout(_timer);
        _timer = setTimeout(() => {
          this.pushBotAndNext();
        }, BOT_MESSAGE_DELAY);
      }
    },
    setUserInfo(type, value) {
      // Possible but not the best approach cause ideally we should not rely on the outer response
      // this.userInfo[type] = value;

      switch (type) {
        case 'name': {
          this.userInfo.name = value;
          break;
        }
        case 'feeling': {
          this.userInfo.feeling = value;
          break;
        }
        case 'location': {
          this.userInfo.location = value;
          break;
        }
        case 'age': {
          this.userInfo.age = value;
          break;
        }
        case 'hobby': {
          this.userInfo.hobby = value;
          break;
        }
        default: {
          console.error(`Can't set type!`);
          // Here we can send type and value to the API to not lose it
          break;
        }
      }
    },
    pushMyMessage(input) {
      this.thread.push({
        "text": input,
        "owner": "me"
      });
    },
    pushBotAndNext() {
      this.thread.push(this.messages[this.next]);
      this.next++;
    }
  }
};
</script>

<style lang="scss" scoped>
.wrapper {
  display: grid;
  grid-template-rows: minmax(auto, calc(100vh - 156px)) 100px;
  box-sizing: border-box;
  padding-bottom: 0;
  padding-top: 0;
}
</style>

<style lang="scss">
body {
  font-family: Helvetica, Arial, sans-serif;
}
</style>
