<template>
  <div>
    <input type="text" placeholder="请输入用户名" v-model="username" />
    <button @click="handEnterBtnClick">进入聊天室</button>
  </div>
</template>

<script>
export default {
  name: 'Login',
}
</script>
<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'

const username = ref('')
const router = useRouter()

onMounted(() => {
  username.value = localStorage.getItem('username')
  if (username.value) {
    router.push('/')
    return
  }
})

const handEnterBtnClick = () => {
  const _username = username.value.trim()
  if (_username.length < 6) {
    alert('用户名长度不能少于6位')
    return
  }
  localStorage.setItem('username', _username)
  username.value = ''
  router.push('/')
}
</script>
