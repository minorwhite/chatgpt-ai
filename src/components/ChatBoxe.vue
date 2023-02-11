<template>
  <div class="ChatBoxe">
    <div class="content">
      <div :class="displayStyle(index)" v-for="(item, index) in msgList" :key="index">
        <img src="../assets/2.png" alt="">
        <div class="con">
          {{ item.msg }}
        </div>
      </div>
    </div>
    <div class="question">
      <input v-model="msg" type="text" @keyup.enter="sendMsg" @confirm="sendMsg" confirm-type="search" placeholder-class="my-neirong-sm"
        placeholder="用一句简短的话描述您的问题" />
      <button @click="sendMsg" :disabled="msgLoad">{{ sentext }}</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ChatBoxe',
  mounted() {
    if (document.cookie != "") {
      this.msgList = JSON.parse(document.cookie);
    }
  },
  data() {
    return {
      // 输入自己的key
      api: 'sk',
      msgLoad: false,
      anData: {},
      sentext: 'SEND',
      animationData: {},
      showTow: false,
      msgList: [{
        my: false,
        msg: "你好我是xavierjade的AI机器人,请问有什么问题可以帮助您?"
      }],
      msgContent: "",
      msg: ""
    }
  },
  methods: {
    // 判断输入方
    displayStyle(index) {
      if (index % 2 == 0) {
        return 'show';
      } else {
        return 'onshow'
      }
    },
    sendMsg() {
      // 消息为空不做任何操作
      if (this.msg == "") {
        alert("请输入内容");
        return 0;
      }
      this.sentext = 'Please Wait'
      this.msgList.push({
        "msg": this.msg,
        "my": true
      })
      console.log(this.msg);
      this.msgContent += ('YOU:' + this.msg + "\n")
      this.msgLoad = true
      // 清除消息
      this.msg = "";
      this.$axios.post('https://api.openai.com/v1/completions', {
        prompt: this.msgContent, max_tokens: 2048, model: "text-davinci-003"
      }, {
        headers: { 'content-type': 'application/json', 'Authorization': 'Bearer ' + this.api }
      }).then(res => {
        console.log(res);
        let text = res.data.choices[0].text.replace("openai:", "").replace("openai:", "").replace(/^\n|\n$/g, "")
        console.log(text);
        this.msgList.push({
          "msg": text,
          "my": false
        })
        this.msgContent += (text + "\n")
        this.msgLoad = false
        this.sentext = 'SEND'
        document.cookie = JSON.stringify(this.msgList);
      })
    },
  }
}
</script>

<style scoped>
.ChatBoxe {
  width: 90vw;
  height: 90vh;
  background: #ffffffa9;
  border-radius: 1%;
  overflow: hidden;
  padding: 1%;
}

.question {
  width: 100%;
  background: #ffffffd8;
  border-radius: 1rem;
  overflow: hidden;
}

.question input {
  width: 80%;
  height: 60px;
  border: none;
  background: none;
  font-size: 1.2rem;
  outline: none;
}

.question button {
  width: 20%;
  height: 60px;
  background-color: #f0f0f0;
  border: none;
  font-size: 1.5rem;
  font-weight: 600;
  color: #fa6fc0;
}

.content {
  height: 90%;
  font-size: 1.2rem;
  overflow: auto;
}

.content img {
  width: 40px;
  height: 40px;
  border-radius: 50%;
}

.show {
  color: #000;
  display: flex;
  margin-bottom: 1rem;
  /* align-items: center; */
}

.onshow {
  display: flex;
  color: #fa6fc0;
  flex-direction: row-reverse;
  margin-bottom: 1rem;
  /* align-items: center; */
}

.con {
  background: #fff;
  padding: 1rem;
  margin: 1rem;
  border-radius: 0px 30px 30px 30px;
}

.show>.con {
  border-radius: 0px 30px 30px 30px;
}

.onshow>.con {
  border-radius: 30px 0px 30px 30px;
}
</style>
