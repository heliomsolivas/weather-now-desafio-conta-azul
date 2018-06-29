<template>
    <div class="weather__wrapper">

        <div class="weather__loader" v-if="loading">
            <img src="/static/img/loader.svg" alt="Loading">
        </div>

        <div class="weather__card" v-for="(weather, index) in weathers" :key="index" v-on:click="activeCard(index)" v-bind:class="{active: index === activeItem}">
            
            <h2 class="weather__card__title">{{weather.name}}, {{weather.sys.country}}</h2>

            <div class="weather__card__degree" v-bind:class="tempColor(Number(weather.main.temp))">
                {{Math.round(weather.main.temp)}}<sup>&#176;</sup>
            </div>

            <div class="weather__card__footer">
                <div class="weather__card__info" v-bind:class="{active: index === activeItem}">
                    <p class="weather__card__info__title">Humidity</p>
                    <p class="weather__card__info__value">{{weather.main.humidity}}<span class="weather__card__info__unit">%</span></p>
                </div>
                <div class="weather__card__info" v-bind:class="{active: index === activeItem}">
                    <p class="weather__card__info__title">Pressure</p>
                    <p class="weather__card__info__value">{{weather.main.pressure}}<span class="weather__card__info__unit">hPa</span></p>
                </div>
                <div class="weather__card__updated">
                    Updated at: {{updatedAt}}
                </div>
            </div>
        </div>
    </div>
</template>

<script>

import axios from 'axios';

export default {
    data(){
        return{
            weathers: '',
            loading: true,
            tempVal: '',
            updatedAt: '',
            activeItem: 1,
        }
    },
    created(){
        this.makeRequest()
        window.clearInterval(this.interval)
        this.interval = window.setInterval(() => this.makeRequest(), 600000);
    },
    methods: {
        activeCard(index){
             this.activeItem = index;
        },
        getActualDate(){
            let d = new Date();
            let currentDate =  d.toLocaleTimeString('pt-BR', {hour12: true});
            console.log(currentDate);
            this.updatedAt = currentDate;
        },
        tempColor(tempValue){
            if(tempValue <= 5){
                return 'blue';
            } else if(tempValue > 26){
                return 'red';
            } else {
                return 'orange';
            }
        },

        makeRequest(){
            axios
            .get('http://api.openweathermap.org/data/2.5/group?id=3421319,3445709,184745&units=metric&APPID=71567a2d63a02c0f1bb2b7eb0dda4c6b')
            .then(res => {
                this.weathers = res.data.list;
                this.loading = false;
                this.getActualDate();
                console.log(this.weathers);
            })
            .catch(error => {
                console.log(error)
                this.errored = true
            })
            .finally(() => this.loading = false)
        }
    },
}
</script>

<style lang="postcss">
    .weather__loader{
        position: fixed;
        top: 50%;
        left: 50%;
        margin-left: -25px;
        margin-top: -25px;
    }
    .weather__card{
        height: 220px;
        border-radius: 5px;
        background-color: #fff;
        margin-bottom:28px;
        box-shadow: 2px 2px 5px 0px rgba(51,51,51,0.1);
        transition: all 0.2s ease-in-out;
            &.active{
                height: 272px;
            }
            &__title{
                height: 42px;
                display: flex;
                align-items: center;
                justify-content: center;
                font-size: 1.8rem;
                color: #737c84;
                border-bottom: 1px solid #ebebeb;
            }
            &__degree{
                height: 138px;
                display: flex;
                align-items: center;
                justify-content: center;
                font-size: 9rem;
                position: relative;
                sup{
                    font-size:5rem;
                    margin-top: -25px;
                }
                &.blue{
                    color: #69A3FF;
                }
                &.orange{
                    color:#ff9632;
                }
                &.red{
                    color: #ed1946;
                }
            }
    }
    .weather__card__footer{
        min-height: 42px;
        background-color: rgba(241, 241, 241, 0.5);
        display: flex;
        flex-wrap: wrap;
    }
    .weather__card__info{
        width: 50%;
        display: none;
        flex-direction: column;
        align-items: center;
        &.active{
            display: flex;
        }
        &__title{
            color: #b4b4b4;
            text-transform: uppercase;
            font-size: 1.2rem;
            padding-top: 14px;
            padding-bottom: 6px;
        }
        &__value{
            color: #737c84;
            font-size: 1.6rem;
        }
        &__unit{
            font-size: 1.1rem;
        }
    }
    .weather__card__updated{
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
        height: 40px;
        color: #b4b4b4;
        font-size: 1rem;
    }

    @media(orientation:landscape){
        .weather__card{
            width: 270px;
            margin: 0 auto 28px;
        }
    }
    @media (min-width: 768px){
        .weather__card{
            width: 270px;
            margin: 0 auto 28px;
        }
    }
    @media (min-width: 890px){
        .weather__wrapper{
            display: flex;
            width: 100%;
            min-height:350px;
            align-items: center;
            justify-content: center;
        }
        .weather__card{
            width: 255px;
            margin: 0 auto 28px;
            transition: all 0.2s ease-in-out;
            &.active{
                transform: scale(1.1);
                transition: all 0.2s ease-in-out;
            }
        }
    }
</style>


