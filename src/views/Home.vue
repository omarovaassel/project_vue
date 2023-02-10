<template>
<!-- важно! у тега template может быть только один дочерний тег -->
    <div class="chats">
        <ChatList/>
        <ChatCreate/>
        <ChatSendMessage/>
        <ChatView/>
        <ChatModal/>
    </div>
</template>
  
<script>
// @ is an alias to /src
// импортируем наши компоненты
import ChatList from '@/components/chat/List.vue'
import ChatCreate from '@/components/chat/Create.vue'
import ChatSendMessage from '@/components/chat/SendMessage.vue'
import ChatView from '@/components/chat/View.vue'
import ChatModal from '@/components/chat/Modal.vue'


// регистрируем наши компоненты
export default {
    name: 'Chat',
        components: {
            ChatCreate,
            ChatList,
            ChatSendMessage,
            ChatView,
            ChatModal
        },
        methods: {

        }
}
</script>

<!-- стили -->
<style scoped lang="less">
@font-face {
  font-family: 'Gordita Light';
  src: local('Gordita Light'), local('Gordita Light'),
  url('@/assets/GorditaLight.woff') format('woff'),
}
* {
    font-family: 'Gordita Light';
  } 
</style>