<template>
  <div class="home">
    <div class="box">
      <h3>Websocket测试</h3>
      <div class="message-box">
        <div v-for="(item, index) in this.websockData" style="padding: 5px 0 5px 0">
          {{item}}
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Home',
  data() {
    return {
      websock: null,
      websockData: []
    }
  },
  created() {
    this.initWebSocket()
  },
  destroyed() {
    this.websock.close()
  },
  methods: {
    initWebSocket() {
      const wsuri = "ws://120.76.142.32:9501/asmp/auth/log/12/websocket";
      this.websock = new WebSocket(wsuri);
      this.websock.onmessage = this.websocketonmessage;
      this.websock.onopen = this.websocketonopen;
      this.websock.onerror = this.websocketonerror;
      this.websock.onclose = this.websocketclose;
    },
    websocketonopen(){ //连接建立之后执行send方法发送数据
      let actions = {"test":"12345"};
      this.websocketsend(JSON.stringify(actions));
    },
    websocketonerror(){//连接建立失败重连
      this.initWebSocket();
    },
    websocketonmessage(e){ //数据接收
      console.log("接收数据：" + e.data);
      this.websockData.push(e.data);
    },
    websocketsend(Data){//数据发送
      this.websock.send(Data);
    },
    websocketclose(e){  //关闭
      console.log('断开连接',e);
    },
  }
}
</script>

<style>
  body {

  }
  .home {
    overflow: hidden;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .box {
    color: white;
    width: 500px;
    height: 600px;
    background-color: dimgray;
    border-radius: 10px;
    box-shadow: 2px 2px 10px #909090;
  }

  h3 {
    padding: 0 0 20px 0;
    border-bottom: solid white 2px;
  }

  .message-box {
    height: 500px;
    width: 500px;
    overflow:auto;
    padding: 0 0 0 25px;
    text-align: left;
  }
</style>