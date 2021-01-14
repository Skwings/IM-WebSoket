<template>
  <div class="Login">
    <input type="text"  id="" placeholder="请输入您要使用的用户名" v-model="username">
    <span>{{this.usernameTip}}</span>
    <button @click="setUsername">进入聊天室</button>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data() {
    return {
      username: '',
      usernameTip: ''
    }
  },
  methods: {
    setUsername(){
      const username = this.username.trim()
      if(username.length > 6){
        usernameTip = '账号名称长度不得大于六位数字！'
      }else if(username == ''){
        this.usernameTip = '账号名称不能为空'
      }

      localStorage.setItem('IMUsername', username);
      this.$router.push('/home')
    }
  },
  mounted() {
    const username = localStorage.getItem('IMUsername')
    if(username){
      this.$router.push('/home')
    }
  },
}
</script>