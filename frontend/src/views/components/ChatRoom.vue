<template>
  <div class="container">
    <main>
      <ul id="chat">
        <li v-for="(m, idx) in msg" :key="idx" v-bind:class="m.style">
          <div class="entete">
            <span class="status green"></span>
            <h2>{{ m.name }}</h2>
            <h3>{{ m.regDate }}</h3>
          </div>
          <!-- <div class="triangle"></div> -->
          <div class="message">
            {{ m.content }}
          </div>
        </li>
      </ul>
      <footer>
        <v-textarea background-color="light-blue lighten-5" v-model="content" placeholder="내용을 입력해주세요" color="orange orange-darken-4"></v-textarea>
        <!-- <textarea v-model="content" placeholder="Type your message"></textarea> -->
        <v-btn color="light-blue lighten-2" style="margin-top:17px; padding: 45px 0px 45px 0px;" dark @click="sendMessage()">전송</v-btn>
      </footer>
    </main>
    <!-- <hr />
    <input type="text" v-model="content" placeholder="보낼 메세지" size="100" />
    <button @click="sendMessage()">SEND</button> -->
  </div>
</template>

<script>
import { createWebChatClient } from '@/api/chat';
export default {
  name: 'ChatRoom',
  data() {
    return {
      title: '',
      content: '',
      client: null,
      msg: [],
      uuid: this.$store.state.uuid,
    };
  },
  props: {
    roomId: {
      type: String,
      required: true,
    },
  },
  mounted() {
    this.client = createWebChatClient(this.roomId, this.$store.state.uuid, this.$store.state.name, this.msg);
    this.title = this.client.getRoomTitle(this.setTitle);
    this.client.getChatBeforeMessage();
    this.client.connect();
  },
  methods: {
    setTitle(title) {
      this.title = title;
    },
    sendMessage() {
      if (this.conent != '') {
        this.client.sendMessage(this.content);
        this.content = '';
      }
    },
  },
};
</script>

<style>
ul {
  max-height: calc(100vh - 48px - 48px);
}

* {
  box-sizing: border-box;
}
body {
  background-color: #abd9e9;
  font-family: Arial;
}
#container {
  width: 750px;
  height: 800px;
  background: #eff3f7;
  margin: 0 auto;
  font-size: 0;
  border-radius: 5px;
  overflow: hidden;
}

main {
  height: 800px;
  display: inline-block;
  font-size: 15px;
  vertical-align: top;
  /* background-color: #abd9e9; */
}

.status {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  display: inline-block;
  margin-right: 7px;
}
.green {
  background-color: #58b666;
}
.orange {
  background-color: #ff725d;
}
.blue {
  background-color: #6fbced;
  margin-right: 0;
  margin-left: 7px;
}

main header {
  height: 110px;
  padding: 30px 20px 30px 40px;
}
main header > * {
  display: inline-block;
  vertical-align: top;
}
main header img:first-child {
  border-radius: 50%;
}
main header img:last-child {
  width: 24px;
  margin-top: 8px;
}
main header div {
  margin-left: 10px;
  margin-right: 145px;
}
main header h2 {
  font-size: 16px;
  margin-bottom: 5px;
}
main header h3 {
  font-size: 14px;
  font-weight: normal;
  color: #7e818a;
}

#chat {
  padding-left: 0;
  margin: 0;
  list-style-type: none;
  overflow-y: scroll;
  height: 535px;
  border-top: 2px solid #fff;
  border-bottom: 2px solid #fff;
}
#chat li {
  padding: 10px 30px;
}
#chat h2,
#chat h3 {
  display: inline-block;
  font-size: 13px;
  font-weight: normal;
}
#chat h3 {
  color: #bbb;
}
#chat .entete {
  margin-bottom: 5px;
}
#chat .message {
  padding: 20px;
  color: #fff;
  line-height: 15px;
  max-width: 85%;
  display: inline-block;
  text-align: left;
  border-radius: 5px;
}
#chat .me {
  text-align: right;
}
#chat .other .message {
  background-color: #58b666;
}
#chat .me .message {
  background-color: #e1f5fe;
  color: black;
}
#chat .triangle {
  width: 0;
  height: 0;
  border-style: solid;
  border-width: 0 10px 10px 10px;
}
#chat .other .triangle {
  border-color: transparent transparent #58b666 transparent;
  margin-left: 15px;
}
#chat .me .triangle {
  border-color: transparent transparent #e1f5fe transparent;
  margin-left: 280px;
}

main footer {
  /* height: 155px; */
  display: flex;
  flex-flow: row wrap;
  /* padding: 20px 30px 10px 20px; */
}
main footer textarea {
  resize: none;
  border: none;
  /* display: block; */
  width: 80%;
  height: 80px;
  border-radius: 3px;
  padding: 20px;
  font-size: 13px;
  margin-bottom: 13px;
}
main footer textarea::placeholder {
  color: #ddd;
}
main footer img {
  height: 30px;
  cursor: pointer;
}
main footer button {
  text-decoration: none;
  text-transform: uppercase;
  font-weight: bold;
  color: #6fbced;
  vertical-align: top;
  /* margin-left: 333px; */
  /* margin-top: 5px; */
  /* display: inline-block; */
}
</style>
