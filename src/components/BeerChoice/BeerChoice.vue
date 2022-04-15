<template>
  <div class="beer-choice__wrapper">
    <div class="beer-choice__content">
      <ul class="beer-info__list">
        <li v-for="item of this.beerInfo" v-bind:key="item" class="beer-info__item"><span><b>{{item.name}}</b>:</span> <span>{{item.data}}</span></li>
      </ul>
    </div>
    <button v-on:click="getBeer">Подобрать пиво</button>
  </div>
</template>

<script>
export default {
  name: "BeerChoice",
  data() {
    return {
      beerInfo: {
        alcohol: {
          name:"Алкоголь",
          data:"Нет данных"
        },
        blg: {
          name:"Экстракт",
          data:"Нет данных"
        },
        brand:{
          name:"Бренд",
          data:"Нет данных"
        },
        hop:{
          name:"Хмель",
          data:"Нет данных"
        },
        ibu:{
          name:"Горечь",
          data:"Нет данных"
        },
        malts:{
          name:"Солод",
          data:"Нет данных"
        },
        name:{
          name:"Название",
          data:"Нет данных"
        },
        style:{
          name:"Сорт",
          data:"Нет данных"
        },
        yeast:{
          name:"Дрожжи",
          data:"Нет данных"
        },
      },
    }
  },
  methods : {
    async getBeer() {
      const getBeerAPI = "https://random-data-api.com/api/beer/random_beer";
      const beer = await fetch(getBeerAPI).then(resp => {
        return resp.json();
      });
      Object.entries(beer).forEach(el=>{
        if(this.beerInfo[el[0]]){
          this.beerInfo[el[0]]["data"] = el[1];
        }

      })
      return beer;
    },
  },
  mounted() {

  }
}
</script>

<style scoped>
.beer-choice__wrapper {
  flex: 1;
  height: 100%;
  display: flex;
  flex-direction: column;
}
.beer-info__item {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.beer-info__item:not(:last-of-type) {
  margin-bottom: 10px;
}

</style>