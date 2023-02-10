<template>
  <div class="wrapper">
    <div class="input">
        <div class="button">+</div>
        <input type="text">        
    </div>

    <div class="list">
      <div class="element"
      v-for="chat in availableChats"
      v-bind:key="chat._id"
      >
      <!-- v-for - это vue атрибут, позволяет нам продублировать какой то элемент, как цикл
      в key мы передаем уникальный идентификатор -->
        <div class="leftContentWrapper">
          <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRl20U-Cn3Bd2KqDtcRya79hyXIH_hRO7T1_w&usqp=CAU">
        </div>
        <div class="rightContentWrapper">
          <div class="title">
            {{ chat.label }}
          </div>
          <div class="lastMessage">
            <div class="from">
                {{ chat.history.at(-1).from }}: &nbsp
                <!-- вытаскиваем последний элемент в массиве, последнее сообщение пользователя в чате -->
            </div>
            <div class="text">
                {{ chat.history.at(-1).text.slice(0, 20) }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import axios from 'axios'

export default {
  name: 'chatList',
  data: () => ({
    serverUrl: 'http://195.49.210.34/',
    availableChats: []
  }),
  async mounted () { // функция вызывается сразу же
    const response = await this.sendRequest('chat', 'GET')
    
    this.availableChats = response.payload
    console.log(this.availableChats[0]);
  },
  methods: {
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
        .wrapper {
          // border: 1px solid rgba(37, 47, 37, 1);
          border-radius: 20px;
          background: linear-gradient(120deg, #17bebb, #f0a6ca);
          margin: 50px;
          height: 80vh;
          padding-top: 20px;
          .input {
            display: flex;  
            width: 38%;
            padding-top: 5px;
            text-align: center;
            align-items: center;
            justify-content: center;
            .button {
              color: white;
              padding-left: 15px;
              text-align: center;
              font-size: 70px;
              cursor: pointer;
            }
            input {
              border: 1px solid white;
              border-radius: 30px;
              width: 100%;
              height: 50px;
              margin-left: 20px;
            }
          }
          .list {
            width: 40%;
            height: 85%;
            padding-top: 5px;
            overflow-y: auto;
            .element {
              height: 80px;
              display: flex;
              justify-content: space-evenly;
              align-items: center;
              margin: 20px;
              cursor: pointer;
              &:hover {
                background-color: rgba(37, 47, 37, .2);
              }
              .leftContentWrapper {
                height: 100%;
                display: flex;
                justify-content: center;
                align-items: center;
                padding: 1.25% 2.5%;
                img {
                  // width: 100% !important;
                  height: 100% !important;
                  border-radius: 50%;
                }
              }
              .rightContentWrapper {
                width: 75%;
                height: 100%;
                display: flex;
                flex-direction: column;
                justify-content: space-evenly;
                align-items: center;
                padding: 7px;
                .title {
                  width: 100%;
                  height: 35%;
                  text-align: left;
                }
                .lastMessage {
                  width: 100%;
                  height: 50%;
                  display: flex;
                }
              }
            }
          }
        }
</style>