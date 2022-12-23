<script>

import axios from 'axios'
export default({
  el: '#app',
  data()
  {
    return {
      nameTable:"",
      id: [],
      name: [],
      password: [],
      dateCreateAccount:[],
      actionList: "",
      inputId: null,
      inputName:null,
      inputPassword:null,
      inputDateCreationAccount:null,
    };
  },
  methods:{
    getRequest(){
      let dataRequest = JSON.stringify({
        "id": this.inputId,
        "nameUser": '\'' + this.inputName + '\'',
        "password": '\'' + this.inputPassword + '\'',
        "dateCreateAccount": '\''+ this.inputDateCreationAccount + '\''
      });
      let config = {
        method: this.actionList,
        url: 'http://25.32.245.49:8080/post?id='+ this.inputId + '&nameUser='+this.inputName+'&password='+ this.inputPassword +'&dateCreateAccount=' + this.inputDateCreationAccount,
  headers: { 
    'Content-Type': 'application/json', 
    'Accept': 'application/json', 
    'Method': 'POST', 
    'Accept-Charset': 'utf8'
  },
  dataRequest : dataRequest
      };  

      this.id = [];
      this.name = [];
      this.password = [];
      this.dateCreateAccount = [];
      if(this.actionList === "all"){
    axios
    .get(`http://25.32.245.49:8080/${this.nameTable}/all`)
    .then(response=>(response.data.forEach(item=>{
      this.id.push(item.id);
      this.name.push(item.nameUser);
      this.password.push(item.password);
      this.dateCreateAccount.push(item.dateCreateAccount);
    })));
  };
  if(this.actionList === "fromKey"){
    axios
    .get(`http://25.32.245.49:8080/${this.nameTable}/fromKey/${this.inputId}`)
    .then(response=>(this.id.push(response.data.id),
    this.name.push(response.data.nameUser),
    this.password.push(response.data.password),
    this.dateCreateAccount.push(response.data.dateCreateAccount)
    ));
    console.log(this.name);
  };
  if(this.actionList==="post"){
    axios(config)
.then(function (response) {
  console.log(JSON.stringify(response.dataRequest));
})
.catch(function (error) {
  console.log(error);
});
  }
  }
}
});
</script>

<template>
  <div class="wrapper container">
    <h1>Приложение для работы с Бд</h1>
    <h1>Таблица {{nameTable }}</h1>
    <div class="container">
      <select name="nameTable" id="nameTable" v-on:change="this.nameTable = $event.target.value">
        <option value="none">--Выбирите таблицу--</option>
        <option value="user">user</option>
        <option value="post">post</option>
        <option value="group">group</option>
        <option value="dialog">dialog</option>
      </select>
    </div>
    <div class="container mt-5">
      <select name="action" id="action" v-on:change="this.actionList=$event.target.value">
        <option value="">--Выбирите действие--</option>
        <option value="all">Получить полный список</option>
        <option value="fromKey">Получить по ключу</option>
        <option value="path">Обновить запись</option>
        <option value="post">Добавить запись</option>
        <option value="delete">Удалить запись</option>
      </select>
    </div>
    <div class="container" v-show="actionList != ''">
      <div class="row mt-5">
        <div class="col">
          <p>Id пользователя</p>
          <input type="text" name="" id="" v-model="inputId">
        </div>
        <div class="col">
          <p>Имя пользователя</p>
          <input type="text" name="" id="" v-model="inputName">
        </div>
        <div class="col">
          <p>Пароль пользователя</p>
          <input type="text" name="" id="" v-model="inputPassword">
        </div>
      </div>
      <div class="row mt-5">
        <div class="col">
          <p>Дата создания аакаунта</p>
          <input type="text" name="" id="" v-model="inputDateCreationAccount">
        </div>
      </div>
      <div class="row mt-5">
        <div class="col">
      <button type="submit" class="btn btn-primary" v-if="nameTable != 'none'" @click="getRequest()">Выполнить</button>
      </div>
      </div>
    </div>
    <div class="container mt-5">
      <div class="row">
      <div class="col">
        <div class="row" v-for="id in id">{{ id }}</div>
      </div>
      <div class="col">
        <div class="row" v-for="name in name">{{ name }}</div>
      </div>
      <div class="col">
        <div class="row" v-for="password in password">{{ password }}</div>
      </div>
      <div class="col">
        <div class="row" v-for="dateCreateAccount in dateCreateAccount">{{ dateCreateAccount }}</div>
      </div>
      </div>
    </div>
  </div>  
</template>

<style scoped>
.wrapper{
  width: 900px;
  height: 100%;
  border-radius: 50px;
  padding: 20px;
  background :midnightblue;
  text-align: center;
  color: whitesmoke;
}

.wrapper h1{
  margin-top: 50px;
}
</style>
