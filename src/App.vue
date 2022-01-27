<template>
  <nav>
    <h2>Covid Tracker</h2>
  </nav>
  <div class="container">
    <h2>{{show ? country.Country : 'Global'}}</h2>
    <div class="data">
      <div class="cases">
        <div>
          <div class="total-cases">
            <p><span>Total Confirmed: </span>{{show ? country.TotalConfirmed : global.TotalConfirmed}}</p>
          </div>
        </div>
      </div>
      <div class="deaths">
        <div>
          <div class="total-deaths">
            <p><span>Total Deaths: </span>{{show ? country.TotalDeaths : global.TotalDeaths}}</p>
          </div>
        </div>
      </div>
    </div>
    <select v-model="val">
      <option v-for="(country, index) in countries" :key="index" :value="country.Country">
        {{country.Country}}
      </option>
    </select>
    <button @click="handleClick">Search</button>
  </div>
</template>

<script>
import { ref } from '@vue/reactivity'

export default {
  name: 'App',
  setup() {
    let show = ref(false);
    let val = ref('');
    let global = ref('');
    let countries = ref('');
    let country = ref('');
    const getData = () => {
      fetch('https://api.covid19api.com/summary')
      .then(res => res.json())
      .then(data => {
        global.value = data.Global;
        countries.value = data.Countries;
      })
      .catch(err => console.log(err));
    }
    getData();
    const handleClick = () => {
      country.value = countries.value.find(element => element.Country === val.value);
      show.value = !show.value;
    }
    return { global, countries, val, handleClick, show, country }
  }
}
</script>

<style>
  *{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  body{
    height: 100vh;
    font-family: sans-serif;
    background: #ccc;
  }
  nav{
    width: 100vw;
    text-align: center;
    background: rgb(18, 18, 214);
    padding: 10px;
    color: #fff;
    letter-spacing: 1px;
  }
  .container{
    width: 90%;
    margin: 20px auto;
  }
  .container .data{
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-auto-rows: 300px;
    gap: 15px;
  }
  .container .data .cases, .container .data .deaths{
    background: #fff;
    border-radius: 10px;
    box-shadow: 10px 10px 10px rgba(0, 0, 0, 0.5);
    display: grid;
    place-items: center;
  }
  .container .data .cases .total-cases p, .container .data .deaths .total-deaths p{
    font-size: 2rem;
  }
  .container .data .cases .total-cases p span, .container .data .deaths .total-deaths p span{
    font-weight: bold;
    font-size: 2rem;
    color: orangered;
  }
  .container select{
    width: 100%;
    border: none;
    padding: 8px;
    background: #fff;
    margin-top: 40px;
  }
  .container button{
    width: 100%;
    border: none;
    padding: 8px;
    background: rgb(13, 184, 13);
    margin-top: 20px;
    color: #fff;
    font-weight: bold;
    letter-spacing: 1px;
    cursor: pointer;
  }
  .container button:hover{
    background: rgb(8, 156, 8);
  }
  .container h2{
    text-align: center;
    margin-bottom: 20px;
  }
  @media(max-width: 800px){
    .container .data{
      grid-template-columns: 1fr;
    }
  }
</style>
