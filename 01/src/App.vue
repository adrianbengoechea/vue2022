<script setup>

import {ref, computed} from 'vue';

console.log('console alive!')

// V-FOR
const name = 'Adrian';
const fontColor = "color: lightblue;";
const fruits = ['Apple', 'Pineapple', 'Banana', 'Orange'];

// COUNTER
const counterDefaultValue = 3 ;
const counter = ref(counterDefaultValue);
const favoriteNumbers = ref([]);
const counterColor = computed(() => {

  if( counter.value > counterDefaultValue ){
    return 'green';

  }else if( counter.value < counterDefaultValue ){
    return 'red';

  }else if( counter.value === counterDefaultValue ){
    return 'white';
  }

});
const disableOnRepeatedFavoriteNumber = computed(() => {
  let disableButton = false;
  if( favoriteNumbers.value.includes(counter.value) ){
    disableButton = true;
  }else{
    disableButton = false;
  }
  return disableButton;
});


const appendToCounter = () => counter.value++
// const minusToCounter = () => (counter.value > 1) ? counter.value-- : false
const minusToCounter = () => counter.value--;
const resetCounter = () => counter.value = counterDefaultValue;
const addToFavorite = () => favoriteNumbers.value.push(counter.value); 

// V-FOR AND V-IF
const hideOutOfStock = false;

// const hideOutOfStock = confirm('Hide out of stock?');
const fruitsNew = 
[
  {
    name: "Manzana",
    price: "$1.00",
    description: "Una manzana",
    stock: 0,
  },
  {
    name: "Pera",
    price: "$2.00",
    description: "Una pera",
    stock: 10,
  },
  {
    name: "Naranja",
    price: "$3.00",
    description: "Una naranja",
    stock: 20,
  },
];

const sendConsoleLog = (message = 'clicked!') => console.log(message)

// V-IF
// const displayUpdatedContent = confirm('Display updated content?');
const displayUpdatedContent = true;

</script>

<template>
  <h1>Hello World! {{name}}</h1>
  <h2 :style="fontColor">I'm blue</h2>

  <hr>

  <h2 v-if="displayUpdatedContent">Updated Content here</h2>
  <h2 v-else>Old Content here</h2>

  <p v-if="displayUpdatedContent">
    Lorem ipsum dolor sit amet consectetur adipisicing elit. Autem minima explicabo sit est? Facere perferendis, itaque, numquam aut dolores esse eaque nulla delectus vitae nostrum nemo deserunt quisquam. Ratione, dolores?
  </p>
  <p v-else>
    Lorem ipsum dolor sit amet consectetur adipisicing elit. Recusandae illum expedita, obcaecati amet, odio exercitationem nisi sapiente ipsa deleniti corporis porro fugit praesentium? Perferendis, temporibus. Ut, autem. Molestias, rerum aliquid.
  </p>

  <hr>

  <ul>
    <li v-for="(fruit, i) in fruits" :key="i">
      {{(i+1) + '. ' +fruit}}
    </li>
  </ul>

  <hr>

  <ul class="list-style-none">
    <template v-for="(fruit, index) in fruitsNew" :key="index">
      <li v-if="!(hideOutOfStock && !fruit.stock)" class="fruit-card">
        <h4 v-for="(value, property, index) in fruit" :key="index">
          <span>{{ property[0].toUpperCase() + property.substring(1).toLowerCase() }}</span>: <span>{{value}}</span>
        </h4>
        <small v-if="!fruit.stock" style="color: red;">Out of stock</small>
        <!-- <h3>{{index + 1}}. {{fruit.name}}</h3>
        <small>{{fruit.price}}</small>
        <p>{{fruit.description}}</p>
        <hr> -->
      </li>
    </template>
  </ul>

  <hr>

  <div>
    <button @click="sendConsoleLog()">Console Log - Default</button>
    <button @click="sendConsoleLog('Custom text here...')">Console Log - Custom Message</button>
    <button @click.middle.prevent="sendConsoleLog()">Console Log - Middle Click</button>
    <button @click.right.prevent="sendConsoleLog()">Console - Log Right Click</button>
  </div>

  <hr>

  <div>
    <!-- NO COMPUTED -->
    <!-- <h3>Counter: <span :style="'color: ' + ( (counter < counterDefaultValue) ? 'red' : ( (counter > counterDefaultValue) ? 'green' : 'white' ) )">{{ counter }}</span></h3> -->

    <!-- COMPUTED -->
    <h3 style="margin-bottom: 10px;">Counter: <span :style="'color: ' + counterColor">{{ counter }}</span></h3>

    <button @click="appendToCounter()">+</button>
    <button @click="resetCounter()">reset</button>
    <button @click="minusToCounter()">-</button>
    <button @click="addToFavorite()" :disabled="disableOnRepeatedFavoriteNumber">Add to favorite</button>

    <br><br>

    <h3 v-if="favoriteNumbers.length">Favorite Numbers:</h3>
    <ul v-if="favoriteNumbers.length">
      <template v-for="(number, index) in favoriteNumbers.sort((a, b) => a - b)" :key="index">
        <li>
          {{ number }}
        </li>
      </template>
    </ul>

  </div>
  <br><br>

</template>

<style>
  @import './assets/main.css';
  #app{
    flex-direction: column;
    display: flex !important;
  }
  h1{
    color: cyan;
    display: block;
  }
  hr{
    margin: 25px 0;
  }
  button{
    padding: 10px 15px;
    border: 1px solid lightcoral;
    background: transparent;
    color: lightcoral;
    margin-right: 8px;
    cursor: pointer;
  }
  button:disabled{
    opacity: 0.5;
    cursor: default;
  }

  .list-style-none{
    list-style: none;
  }
  .fruit-card{
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    margin-bottom: 20px;
  }
  .fruit-card hr{
    border: 0;
    border-bottom: 1px solid rgba(255,255,255,0.4);
    margin: 10px 0;
  }
</style>