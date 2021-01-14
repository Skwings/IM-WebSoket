<template>
  <div class="home">
    <button @click="logOut" class="logOutBtn">注销</button>
    <p>{{this.username}}，欢迎您来到Skwings聊天室</p>

    <input type="text" placeholder="请输入消息" id="" v-model="msg" @keyup.enter="handleSendBtnClick">
    <button @click="handleSendBtnClick">发送</button>
    <ul>
      <li v-for="item of msgList" :key="item.id">
        <p>
          <span>{{new Date(item.dateTime)}}</span>  
        </p>
        <p>
          <span>{{item.username}}:</span>
          <span>{{item.msg}}</span>
        </p>
      </li>
    </ul>
  </div>
</template>

<script>

const ws = new WebSocket('ws://localhost:8000');

export default {
  name: 'Home',
  data() {
    return {
      msg: '',
      username: '', //用户名
      msgList: [] //消息列表
    }
  },
  methods: {
    handleSendBtnClick(){ //发送消息
      const msg = this.msg;
      if(!msg.trim().length){
        return
      }
      ws.send(JSON.stringify({
        id: new Date(),
        username: this.username,
        dateTime: new Date().getTime(),
        msg: this.msg
      }))
    },
    logOut(){ //注销
      localStorage.removeItem('IMUsername');
      this.$router.push('/login')
    },

    handleWsOpen(e){
      console.log('FE: Websocket open', e)
    },
    handleWsClose(e){
      console.log('FE: Websocket close', e)
    },
    handleWsError(e){
      console.log('FE: Websocket error', e)
    },
    handleWsMsg(e){
      console.log('FE: Websocket message', e)
      this.msgList.push(JSON.parse(e.data) );
    }
  },
  mounted() {
    this.username = localStorage.getItem('IMUsername')

    if(!this.username){
      this.$router.push('/login');
      return
    }

    ws.addEventListener('open', this.handleWsOpen.bind(this) , false)
    ws.addEventListener('close', this.handleWsClose.bind(this), false)
    ws.addEventListener('error', this.handleWsError.bind(this), false)
    ws.addEventListener('message', this.handleWsMsg.bind(this), false)
  },
}
</script>

<style lang="scss" scoped>
.home{
  position: relative;
  .logOutBtn{
    cursor: pointer;
  }
}
</style>
