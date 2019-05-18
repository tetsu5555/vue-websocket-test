<template>
  <div>
    <h2>RealTimeChat</h2>
    <!-- 投稿フォーム -->
    <form @submit.prevent="sendMessage">
      名前: <input v-model="name" type="text" />
      投稿内容: <input v-model="message" type="text" />
      <button type="submit">送信</button>
    </form>
    <!-- チャットの表示 -->
    <div v-for="(row,index) in messages" :key="index">{{ row.name }}: {{ row.message }}</div>
  </div>
</template>

<script>
  import io from 'socket.io-client';

  export default {
    name: 'Index',
    data: () => ({
      name: '',
      message: '',
      messages: []
    }),
    methods: {
      // チャットを投稿する処理
      sendMessage(e) {
        e.preventDefault();
        io('localhost:3000').emit('POST_MESSAGE', {
            name: this.name,
            message: this.message
        });
        this.message = ''
      },
    },
    mounted(){
      // 投稿されたデータの取得
      io('localhost:3000').on('MESSAGE', (data) => {
        this.messages = [...this.messages, data];
      })
    },
}
</script>

<style scoped>
</style>

