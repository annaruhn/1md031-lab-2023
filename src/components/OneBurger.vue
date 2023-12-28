<template>
<div>
  <h3>{{ burger.name }}</h3>
  <br>
  <img v-bind:src="burger.img" alt="BurgerPic" title="ImageOfBurger" style="width: 100%;">
  <ul>
    <li>{{ burger.kCal }} kCal</li>
    <li v-if = "burger.chillies"><img v-for="index in burger.chillies" :key="index" src="https://upload.wikimedia.org/wikipedia/commons/2/2b/Hot_chili_red_pepper_icon_emote.png" alt="Chilies" title="ImageOfChillies" style="width: 7%;"></li>
  </ul>
  <section class="ingredient">
    <ul>
      
      <li v-if = "burger.lactose"> Contains lactose </li>
      <li v-if = "burger.gluten"> Contains gluten </li>
    </ul>
  </section>
  <br>
  <button v-on:click = "increaseAmount(key)" type="number">
                +
              </button>
              <span>Quantity: {{ amountOrdered }}</span>
   <button v-on:click = "decreaseAmount(key)" type="number">
                -
              </button>
 
</div>
  </template>
  
<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object
  },
  
data: function () {
  return {
    amountOrdered: 0
  }
},

methods: {  
  increaseAmount: function() {
    this.amountOrdered++;
    this.$emit('orderedBurgers', { name:   this.burger.name, 
                                  amount: this.amountOrdered }
              );
  },
  decreaseAmount: function() {
    this.amountOrdered--;
    this.$emit('orderedBurgers', { name:   this.burger.name, 
                                  amount: this.amountOrdered }
              );
  }
}
}
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  
 section div {
    padding: 2em;
    margin: 1em;
    background-color:black;
}

 #burger div h3{
     margin: 0 auto 0.5em auto;
     font-size: 2em; 
     font-weight: bold;
}

 #burger div li{
    margin: 0.5em auto auto 1em;
    font-size: 1em;
    list-style-type: disc;
} 
  </style>
  