<template>
  <div id="app">
      <div id="wrapper">
          <b-container>
              <h1>{{gifts.title}}</h1>
              <div class="parent">
                  <b-button v-for="day in daysInMonth(currentMonth,currentFullYear)" class="days"
                            :key="day" @click="showModal(day)" :disabled="isDisabled(day)">
                      <div class="day">{{day}}</div>
                      <div class="month">{{month}}</div>
                  </b-button>
              </div>
              <b-modal ref="my-modal" size="lg">
                  <h3>{{gifts.nameList}}</h3>
                  <b-row v-for="i in maxGifts">
                      <b-col cols="1">
                          <div class="text">{{i}}</div>
                      </b-col>
                      <b-col class="text">{{currentGift['text' + i]}}</b-col>
                  </b-row>
              </b-modal>
          </b-container>
      </div>
  </div>
</template>

<script>
import gifts from './../public/gifts.json';

export default {
  name: 'App',
    data(){
      return{
          gifts:gifts,
          maxGifts:1,
          currentMonth : new Date().getMonth(),
          currentFullYear : new Date().getFullYear(),
          month: '',
          currentGift:{
              text1:'',
              text2:'',
              text3:'',
              text4:'',
          },
      }
    },
    created(){
        this.month = this.SearchMonth(this.currentMonth);
        let allLength = [];
        for(let object of this.gifts.gifts){
            allLength.push(Object.keys(object).length)
        }
        this.maxGifts =  Math.max.apply(null, allLength)
    },
    methods: {
        showModal(day) {
            for (let i in this.gifts.gifts){
                if(this.gifts.gifts.length > day-1) {
                    this.currentGift['text' + i] = this.gifts.gifts[day - 1]['text' + i];
                }
                else{
                    this.currentGift['text'+i] = '';
                }
            }
            this.$refs['my-modal'].show();
        },
        isDisabled(day){
            const currentDay = new Date().getDate();
            if(day > currentDay || this.currentFullYear === 2022){
                return true;
            }
        },
        daysInMonth(currentMonth,currentFullYear) {
            return 33 - new Date(currentFullYear, currentMonth, 33).getDate();
        },
        SearchMonth: function (numberMonth) {
            const months = {
                0: "января",
                1: "февраля",
                2:"марта",
                3:"апреля",
                4:'мая',
                5:'июня',
                6:'июля',
                7:'августа',
                8:'сентября',
                9:'октября',
                10:'ноября',
                11:'декабря',
            }
            return months[numberMonth];
        },
    },
}
</script>

<style lang="scss">
    #wrapper {
        background-image: url('./../public/snowflake.png'), url('./../public/snowflake2.png'), url('./../public/snowflake3.png');
        height:100%;
        animation: snow 10s linear infinite;
    }
    @keyframes snow {
        0% {background-position: 0 0, 0 0, 0 0;}
        100% {background-position: 500px 1000px, 400px 400px, 300px 300px;}
    }
    #app {
        min-height: 100vh;
        height: auto;
        margin: 0 !important;
        padding: 0 !important;
        background: url("./../public/bg.jpg") no-repeat ;
        background-size: 100% 100%;
    }
    .container{
        padding-right: 15px !important;
        padding-left: 15px !important;
    }
    h1{
        color: white;
        padding-top: 2%;
        padding-bottom: 1%;
    }
    h1,h3{
        text-align: center;
        font-family: 'EB Garamond', serif;
    }
    .row{
        padding-bottom: 1%;
        .col-1 div{
            min-width: 25px;
            width: 90%;
            min-height: 30px;
            color: #FFFFFF;
            font-weight: 500;
            text-shadow: #212529 1px 0 4px;
        }
        &:nth-child(2) .col-1 div{
            background: rgba(123, 22, 22, 0.5) linear-gradient(rgba(245, 205, 213, 0.4), rgba(141, 130, 56, 0.3));
        }
        &:nth-child(3) .col-1 div{
            background: rgb(124 106 7 / 60%) linear-gradient(rgba(234, 232, 224, 0.38), rgba(234, 234, 226, 0.29))
        }
        &:nth-child(4) .col-1 div{
            background: rgba(48, 123, 22, 0.5) linear-gradient(rgba(205, 245, 225, 0.35), rgba(79, 141, 56, 0.25));
        }
    }
    .col-1{
        color: #850f0e;
        font-family: 'EB Garamond', serif;
        text-align: center;
        font-size: 2rem;
        div{
            color: white;
            border-radius: 50%;
        }
    }
    .col{
        font-family: 'EB Garamond', serif;
        font-size: 2rem;
    }
    .btn:focus{
        background: rgba(123, 22, 22, 0.5) linear-gradient(rgba(245, 205, 213, 0.4), rgba(141, 130, 56, 0.3)) !important;
    }
    .text{
        font-size: 1.6rem;
    }
    .parent {
        padding: 0 15% 10%;
        display: grid;
        grid-template-columns: repeat(6, 1fr);
        grid-template-rows: repeat(7, 1fr);
        grid-column-gap: 6px;
        grid-row-gap: 6px;
        .days[disabled]{
            background: #ccc !important;
        }
        .days{
            border-radius: 6px;
            box-shadow: 0 2px 15px 2px #3d3b19;
            color: #FFFFFF;
            font-weight: 500;
            text-shadow: #212529 1px 0 4px;
            font-family: 'EB Garamond', serif;
            display: flex;
            align-items: center;
            border: 2px solid rgba(255, 215, 0, 0.22);
            background: rgba(255, 215, 0, 0.22) linear-gradient(rgba(234, 232, 224, 0.38), rgba(234, 234, 226, 0.29));
            justify-content: center;
            &:hover{
                background: rgba(48, 123, 22, 0.5) linear-gradient(rgba(205, 245, 225, 0.35), rgba(79, 141, 56, 0.25));
            }
            .day{
                font-size: 2.5rem;
                padding-right: 5%;
            }
            .month{
                font-size: 1.25rem;
            }
            &:first-child{
                grid-area: 1 / 1 / 2 / 3;
            }
            &:nth-child(2){
                grid-area: 1 / 3 / 2 / 4;
            }
            &:nth-child(3){
                grid-area: 1 / 4 / 2 / 5;
            }
            &:nth-child(4){
                grid-area: 1 / 5 / 3 / 6;
            }
            &:nth-child(5){
                grid-area: 1 / 6 / 2 / 7;
            }
            &:nth-child(6){
                grid-area: 2 / 1 / 3 / 2;
            }
            &:nth-child(7){
                grid-area: 2 / 2 / 3 / 3;
            }
            &:nth-child(8){
                grid-area: 2 / 3 / 3 / 5;
            }
            &:nth-child(9){
                grid-area: 2 / 6 / 3 / 7;
            }
            &:nth-child(10){
                grid-area: 3 / 1 / 4 / 3;
            }
            &:nth-child(11){
                grid-area: 3 / 3 / 4 / 4;
            }
            &:nth-child(12){
                grid-area: 3 / 4 / 4 / 5;
            }
            &:nth-child(13){
                grid-area: 3 / 5 / 4 / 6;
            }
            &:nth-child(14){
                grid-area: 3 / 6 / 4 / 7;
            }
            &:nth-child(15){
                grid-area: 4 / 1 / 6 / 2;
            }
            &:nth-child(16){
                grid-area: 4 / 2 / 5 / 3;
            }
            &:nth-child(17){
                grid-area: 4 / 3 / 5 / 4;
            }
            &:nth-child(18){
                grid-area: 4 / 4 / 5 / 5;
            }
            &:nth-child(19){
                grid-area: 4 / 5 / 5 / 6;
            }
            &:nth-child(20){
                grid-area: 4 / 6 / 5 / 7;
            }
            &:nth-child(21){
                grid-area: 5 / 2 / 6 / 3;
            }
            &:nth-child(22){
                grid-area: 5 / 3 / 6 / 4;
            }
            &:nth-child(23){
                grid-area: 5 / 4 / 6 / 6;
            }
            &:nth-child(24){
                grid-area: 5 / 6 / 6 / 7;
            }
            &:nth-child(25){
                grid-area: 6 / 1 / 7 / 3;
            }
            &:nth-child(26){
                grid-area: 6 / 3 / 7 / 4;
            }
            &:nth-child(27){
                grid-area: 6 / 4 / 7 / 6;
            }
            &:nth-child(28){
                grid-area: 6 / 6 / 7 / 7;
            }
            &:nth-child(29){
                grid-area: 7 / 1 / 8 / 3;
            }
            &:nth-child(30){
                grid-area: 7 / 3 / 8 / 5;
            }
            &:nth-child(31){
                grid-area: 7 / 5 / 8 / 7;
            }
        }
    }
    .modal.show .modal-dialog {
        margin-top: 14% !important;
    }
    .modal-body {padding: 0 1rem 1rem 1rem!important;}
    .modal-footer {display: none !important;}
    .close {
        border: none !important;
        font-size: 1.5rem;
        background: transparent;
    }
    .modal-header{
        background: #e9ecef;
        padding: .3% !important;
    }
    @media screen and (min-width: 768px) and (max-width: 1200px) {
        .parent{
            padding: 0 0 10%;
            .days {
                padding: 3px;
                .day {
                    font-size: 2rem;
                }
                .month {
                    font-size: 1.25rem;
                }
            }
        }
        .text,.close{
            font-size: 1.4rem;
        }
    }
    @media screen and (min-width: 500px) and (max-width: 768px) {
        .parent{
            padding: 0 0 10%;
            grid-column-gap: 3px;
            grid-row-gap: 3px;
            .days {
                display: block;
                padding: 0;
                .day {
                    padding-right: 0;
                    font-size: 1.25rem;
                }
                .month {
                    font-size: 1.15rem;
                }
            }
        }
        .text,.close{
            font-size: 1.25rem;
        }
    }
    @media screen and (min-width: 400px) and (max-width: 500px) {
        .modal.show .modal-dialog {
            margin-top: 25% !important;
        }
        .parent{
            padding: 0 0 10%;
            grid-column-gap: 3px;
            grid-row-gap: 3px;
            .days {
                display: block;
                box-shadow: 0 2px 2px 2px #3d3b19;
                padding: 6% 0;
                border: 1px solid rgba(255, 215, 0, 0.22);
                .day {
                    padding-right: 0;
                    font-size: 1.25rem;
                }
                .month {
                    font-size: 1rem;
                }
            }
        }
        .text,.close{
            font-size: 1.25rem;
        }
    }
    @media screen  and (max-width: 400px) {
        h1{

        }
        .modal.show .modal-dialog {
            margin-top: 35% !important;
        }
        .parent{
            padding: 0 0 10%;
            grid-column-gap: 2px;
            grid-row-gap: 2px;
            .days {
                display: block;
                box-shadow: 0 2px 2px 2px #3d3b19;
                padding: 6% 0;
                border: 1px solid rgba(255, 215, 0, 0.22);
                .day {
                    padding-right: 0;
                    font-size: 1.25rem;
                }
                .month {
                    font-size: .8rem;
                }
            }
        }
        .text,.close{
            font-size: 1rem;
        }
    }
    @media screen  and (max-width: 300px) {
        .parent{
            .days {
                .month {
                    display: none;
                }
            }
        }
        .text,.close{
            font-size: .8rem;
        }
    }
</style>
