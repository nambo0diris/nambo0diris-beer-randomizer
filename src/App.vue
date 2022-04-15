
<template>
  <div class="wrapper">
    <div class="header">
      <div class="container"><h1>BeeRandomizer</h1></div>
    </div>
    <div class="profile">
      <div class="container">
       <div class="row">
         <div class="profile__photo">
           <img v-bind:src=this.userinfo.imgUrl alt="">
         </div>
         <ul class="profile__info-list">
           <li class="profile__info-item"><b>Имя:</b> {{this.userinfo.name}}</li>
           <li class="profile__info-item"><b>Возраст:</b> {{this.userinfo.age}}</li>
           <li class="profile__info-item"><b>Должность:</b> {{this.userinfo.status}}</li>
         </ul>
       </div>
      </div>
    </div>
    <div class="main">
      <div class="container">
        <div class="row row__flex-start">
          <div class="bar__wrapper">
            <MyCanvas2/>
          </div>
          <BeerChoice/>
        </div>
      </div>
    </div>
    <div class="footer">
      <div class="container">Решаю пивные споры с 2018 года.</div>
    </div>
  </div>
</template>




<script>

// import MyCanvas from "@/components/MyCanvas/MyCanvas";
import BeerChoice from "@/components/BeerChoice/BeerChoice";
import ava from "./assets/images/ava.jpg";
import MyCanvas2 from "@/components/MyCanvas/MyCanvas2";

export default {
  name: 'App',
  components: {BeerChoice, MyCanvas2},
  data() {
    return {
      userinfo: {
        imgUrl:ava,
        name:"Нет данных",
        age:"Нет данных",
        status:"Нет данных",
      }
    }
  },
  mounted() {
    this.getUser();
  },
  methods: {
    getAge(date){
      let now = new Date(); //Текущя дата
      let today = new Date(now.getFullYear(), now.getMonth(), now.getDate()); //Текущя дата без времени
      let dob = new Date(date); //Дата рождения
      let dobnow = new Date(today.getFullYear(), dob.getMonth(), dob.getDate()); //ДР в текущем году
      let age; //Возраст

      //Возраст = текущий год - год рождения
      age = today.getFullYear() - dob.getFullYear();
      //Если ДР в этом году ещё предстоит, то вычитаем из age один год
      if (today < dobnow) {
        age = age-1;
      }
      return age;
    },
    async getUser() {
      const getUserAPI = "https://random-data-api.com/api/users/random_user";
      const user = await fetch(getUserAPI).then(resp => {
        return resp.json();
      });
      this.userinfo.name = user['first_name'] + " " + user['last_name'];
      this.userinfo.age = this.getAge(user['date_of_birth']);
      let image = user['avatar'] ? user['avatar'] : ava;
      this.userinfo.imgUrl = image;
      this.userinfo.status = user['employment']['title'] ? user['employment']['title'] : "Безработный";
      return user;
    },
  },
}



</script>
<style lang="scss">
  @import "assets/styles/normalize";
  @import "assets/styles/style";
</style>