<template>
  <div>
    Burgers
    <Burger v-for="burger in burgers"
            v-bind:burger="burger" 
            v-bind:key="burger.name"/>
  </div>
  <div id="map" v-on:click="addOrder">
    click here
  </div>
</template>

<script>
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'

const socket = io();

function MenuItem(Name,url,kCal,lactose,gluten) {
  this.name = Name; // The *this* keyword refers to the object itself
  this.pic = url;
  this.calories = kCal;
  this.contents = lactose;
  this.ingrediens = gluten;
  return this
}

const BurgerArray = [
    new MenuItem("Jappaleno","https://www.capetown.travel/wp-content/uploads/2018/12/Twelve_Apostles_Hotel_Views.jpg",800,true,true),
    new MenuItem("BigFille","https://silveroak.com/wp-content/uploads/2020/03/Recipe-Wagyu-Burger.jpg",1200,false,true),
    new MenuItem("Meastrospecial","https://537924-1719237-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2021/02/Classic-homemade-hamburger-1024x683.jpg",1100,false,false)
];
console.log(BurgerArray);

export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      yourVariabel:"Välj en burgare",
      burgers: BurgerArray
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
    }
  }
}
</script>

<style>
  #map {
    width: 300px;
    height: 300px;
    background-color: red;
  }
</style>
