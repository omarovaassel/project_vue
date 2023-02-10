<template>
    <div class="wrapper">
        <div class="close_modal">x</div>
        <!-- чтобы вытащить значение value из инпута пишем v-model -->
        <input v-model="label" placeholder="Название">
        <input v-model="description" placeholder="Описание">

        <div class="members">Выберите участников чата</div>
        <div class="members_block">
            <input v-model="selectMember" class="members_list" placeholder="Участники">
            <div class="members_button" v-on:click="add_member_button_handler">Добавить</div>
        </div>        
        
        <div class="members_list_choose">Вы выбрали: {{ selectedMembers }}</div>

        <div class="new_chat_button" v-on:click="create_chat_button_handler">Создать чат</div>        
    </div>
</template>
  
  
<script>
import axios from 'axios'
export default {
    name: 'createChat',
    // в data мы объявляем все переменные
    data: () => ({
        label: '',
        description: '',
        selectMember: '',
        selectedMembers: [],
        serverUrl: 'http://195.49.210.34/',
    }),
    methods: {
        create_chat_button_handler: async function () {
        // проверка
        if(!this.label || !this.description) return alert('Вы не заполнили поля "Название" или "Описание"')
        
        // собираем все данные для создания чата
        const request_body = {
            userId: this.$route.params.id || "63e253d392021356e711e486", // id пользователя который хочет создать новый чат
            label: this.label, // название чата
            members: this.selectedMembers, // массив участников чата
            description: this.description // описание чата, необязательный параметр!
        }

        // отправляем запрос на создание чата
        const response = await this.sendRequest('chat/create', "POST", request_body);
        
        // проверяем удалось ли создать чат
        console.log({response});
    },
    
    add_member_button_handler: async function () {
        // делаем проверку для пользователя
        if (!this.selectMember) return alert('Вы не выбрали участника');
        if (this.selectedMembers.includes(this.selectMember)) return alert('Этого участника вы уже выбрали');
            
        // сюда отправляем массив this.selectedMembers, в котором будет последнее значение массива selectMember
        this.selectedMembers.push(this.selectMember);
        this.selectMember = ''
    },
    sendRequest: async function (path, method, body) {
        const url = `${this.serverUrl}${path}`;
        
        if (method === 'GET') {
            return (await axios.get(url)).data
        } else {
            return (await axios.post(url, body)).data
        }

        // сокращенный вариант того же самого кода
        // axios - это какой то объект, у которого есть методы
        // method = method.toLowerCase()
        // return await axios[method](url).data
    }
}
}
</script>
    
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
@font-face {
    font-family: 'Gordita Light';
    src: local('Gordita Light'), local('Gordita Light'),
    url('~@/assets/GorditaLight.woff') format('woff'),
}
* {
    font-family: 'Gordita Light';
    font-size: 17px;
    margin: 8px;
}
.wrapper {
    border: 1px solid #17bebb;
    border-radius: 20px;
    background-color: white;
    margin: 50px;
    padding: 20px;
    height: auto;
    width: 500px;
    // padding-top: 20px;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    text-align: center;
    position: relative;
    .close_modal {
        text-align: right;
        margin-top: -15px;
    }
    .members_block {
        display: flex;
    }
    .members_button, .new_chat_button {
        border-radius: 20px;
        background: linear-gradient(120deg, #17bebb, #f0a6ca);
        padding: 5px;
        text-align: center;            
        cursor: pointer;
        color: white;
        font-weight: bold;
    }
    .members_list {
        width: 70%;
        padding: 5px;
    }
    .members_button {
        width: 20%;
        padding: 5px;
    }
    input {
        // display: flex;  
        // width: 100%;
        text-align: center;
        border: 1px solid #17bebb;
        border-radius: 20px;
        padding: 5px;
    }
}
</style>