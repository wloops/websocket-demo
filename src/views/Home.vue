<template>
  <div>
    <ul>
      <li
        v-for="(item, index) in msgList"
        :key="item.id"
      >
        <p>
          <span>{{item.user}}</span>
          <span>{{new Date(item.dateTime)}}</span>
        </p>
        <p>消息：{{item.msg}}</p>
      </li>
    </ul>
    <input type="text" placeholder="请输入消息"  v-model="msg"/>
    <button @click="sendMsg">发送消息</button>
  </div>
</template>

<script>
import { onMounted,ref,reactive,toRefs } from 'vue'
import { useRouter }from 'vue-router'
import {useWebSocket} from '@/hooks'
export default {
  name: 'Home',
  setup() {

const router = useRouter()
const ws = useWebSocket(handleMessage)
const state =  reactive({
  msg: '',
  msgList: []
})

let username = ''
onMounted(() =>{
  username = localStorage.getItem('username')
  if(!username){
    router.push('/login')
  }
})

const sendMsg = () => {
  const _msg = state.msg

  if (!_msg.trim().length){
    return
  }

  // state.msgList.push()

  ws.send(JSON.stringify({
    id: new Date().getTime(),
    user: username,
    dateTime: new Date().getTime(),
    msg: state.msg
  }))

  state.msg = ''
}

function handleMessage(e) {
  const _msgData = JSON.parse(e.data)
  state.msgList.push(_msgData)
}

  return {
    ...toRefs(state),
    sendMsg
  }
  }
}
</script>
