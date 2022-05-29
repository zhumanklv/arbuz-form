<template>
  <div class="container main-container">
    <h2 class="text-center my-4">Форма для покупки</h2>
    <form class="container" @submit="handleSubmitform($event)">
      <div class="d-flex flex-wrap">
        <div v-for="(item, index) in watermelons" :style="{backgroundColor: selected.includes(index) ? '#a5b9b9' : ''}" class="watermelon-item" @click="handleWatermelonClick(index)">
          <img src="./assets/watermelon.svg" class="watermelon-icon">
          <div>Масса: {{item.mass}} кг</div>
          <div>{{item.isRipe=== 'спелая' ? 'Спелая' : item.isRipe === 'не спелая' ? 'Еще не спело' : 'Уже сорвано'}}</div>
        </div>
      </div>
      <div v-if="selected.length > 0">
        <div class="text-center" style="font-size: 30px">Корзина</div>
        <div class="selected-watermelons">
          <div class="selected-watermelons--inner">
            <img src="./assets/watermelon.svg" class="watermelon-icon">
            <div>Общая масса:  {{totalMass}}кг</div>
            <div>Количество: {{selected.length}}</div>
          </div>
        </div>
      </div>
      <div class="contact mb-2">
        <label for="con">Контактный номер</label>
        <div class="contact-input-container">
          <div class="contact-begin"><div>+7</div></div>
          <input v-model="phoneNumber" class="form-control contact-input" id="con" type="tel"  maxlength="13" placeholder="(___) ___-____"/>
        </div>
      </div>
      <h2>Адрес</h2>
      <div>Город</div>
      <select class="select-city form-select mb-2">
        <option>Алматы</option>
        <option>Нур-Султан</option>
      </select>
      <div>Улица, Дом</div>
      <input class="form-control mb-2"/>
      <h4>Дата доставки</h4>
      <div class="d-flex dates-outer mb-2">
        <div @click="handleDateClick(index)" class="mx-1 p-2 date" :style="{backgroundColor: index !== selectedDate ? '' : '#28a745', color: index !== selectedDate ? '' : 'white'}" v-for="(date, index) in dates">
          <span class="date-span">{{date}}</span>
        </div>
      </div>
      <h4>Время доставки</h4>
      <div class="d-flex dates-outer mb-2">
        <div @click="handleTimeClick(index)" class="mx-1 p-2 date" :style="{backgroundColor: index !== selectedTime ? '' : '#28a745', color: index !== selectedTime ? '' : 'white'}" v-for="(time, index) in deliveryTime.slice(currentHour)">
          <span class="date-span">{{time}}</span>
        </div>
      </div>
      <div>
        <label>
          <h5>Порезать дольками</h5>
          <input type="checkbox"/>
        </label>
      </div>
      <button class="submit-button" @click="handleSubmitform">
        ОФОРМИТЬ ЗАКАЗ
      </button>
    </form>
  </div>
</template>

<script>
import watermelons from './assets/watermelons.js'
 export default {
   data() {
     const currentHour = new Date().getHours() - 8;
     console.log('currentHour', currentHour);
     const months = ['Января', 'Февраля', 'Марта', 'Апреля', 'Мая', 'Июня', 'Июля', 'Августа', 'Сентября', 'Октября', 'Ноября', 'Декабря'];
     const dates = [];
     for (let i = 0; i < 10; i++) {
       const currentDate = new Date(Date.now() + 86400000 * i);
       dates.push((i === 0 ? 'Сегодня, ' : '') + currentDate.getDate() + ' ' + months[currentDate.getMonth()]);
     }
     return {
        currentHour: currentHour,
        selectedTime: 0,
        selectedDate: 0,
        dates: dates,
        deliveryTime: ['09:00 - 11:00', '10:00 - 12:00', '11:00 - 13:00', '12:00 - 14:00', '13:00 - 15:00', '14:00 - 16:00', '15:00 - 17:00', '16:00 - 18:00', '17:00 - 19:00', '18:00 - 20:00', '19:00 - 21:00', '20:00 - 22:00'],
        phoneNumber: '',
        totalMass: 0,
        selected: [],
        watermelons,
     }
   },
   watch: {
     selectedDate() {
       if (this.selectedDate === 0) {
         this.currentHour = new Date().getHours() - 8;
       } else {
         this.currentHour = 0;
       }
     },
     phoneNumber(newValue, oldValue) {
       if (newValue.length > oldValue.length) {
         if (this.phoneNumber.length === 1) {
           this.phoneNumber = '(' + this.phoneNumber;
         } else if (this.phoneNumber.length === 4) {
           this.phoneNumber = this.phoneNumber + ')';
         } else if (this.phoneNumber.length === 8) {
           this.phoneNumber += '-';
         }
       }
     }
   },
   methods: {
     handleDateClick(index) {
       this.selectedDate = index;
     },
     handleTimeClick(index) {
       this.selectedTime = index;
     },
     handleWatermelonClick(index){
       if (this.selected.includes(index)) {
         this.totalMass -= this.watermelons[index].mass;
         this.selected = this.selected.filter(item => item !== index);
         return;
       }
       if (this.selected.length >= 3) {
          return;
        } else if (this.watermelons[index].isRipe === 'не спелая' || this.watermelons[index].isRipe === 'уже сорвано') {
          return;
        }
        this.totalMass += this.watermelons[index].mass;
        this.selected.push(index);
     },
     handleSubmitform(e) {
       e.preventDefault();
     }
   }
 }
</script>
<style>
@import "./assets/base.css";
.main-container{
  max-width: 800px !important;
}
.watermelon-item{
  border-radius: 3px;
  padding: 6px;
  width: 120px;
  cursor: pointer;
  margin: 10px;
}
.watermelon-item:hover{
  background-color: #a5b9b9;
}
.watermelon-item:hover > .watermelon-icon{
  background-color: #a5b9b9;
}
.watermelon-icon{
  width: 50px;
  height: 50px;
}
.selected-watermelons{
  width: 220px;
  margin: 20px auto;
  padding: 10px;
  background-color: aqua;
  border-radius: 7px;
  font-size: 20px;

  display: flex;
  justify-content: center;
}
.selected-watermelons--inner{
  background-color: aquamarine;
  padding: 10px;
  border-radius: 7px;

  display: flex;
  flex-direction: column;
  align-items: center;
}
.contact-input-container{
  display: flex;
  align-items: center;
  width: 200px;
}
.contact-begin{
  background-color: #aeb7be;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 38px;
  height: 38px;

  border-bottom-left-radius: 4px;
  border-top-left-radius: 4px;
}
.contact-input{
  outline: none;
  border-radius: unset !important;
}
.dates-outer{
  width: 1500px;
}
.date{
  width: max-content;
  box-shadow: 0 1px 0 0 rgb(0 50 0 / 15%);
  border-radius: 5px;
  border: 0.8px solid #eff5f5;
  cursor: pointer;
}
.date-span{
  width: max-content;
}
.submit-button{
  background-color: #28a745;
  width: 100%;
  text-align: center;
  outline: none;
  border: none;
  border-radius: 5px;
  margin-top: 12px;
  padding: 10px;
  color: white;
}
h5 {
  display: inline-block;
  margin-right: 10px;
}
@media (max-width: 600px) {
  .main-container{
    max-width: 600px !important;
  }
  .watermelon-item{
    width: 80px;
    font-size: 12px;
  }
  .watermelon-icon{
    width: 35px;
    height: 35px;
  }
}
</style>
