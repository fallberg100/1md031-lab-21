<template>
  <header class="format">
    <div id="container">
      <img id="headerimage" src="https://www.capetown.travel/wp-content/uploads/2018/12/Twelve_Apostles_Hotel_Views.jpg">
      <h1 id="headertext"> Welcome to BurgerOnline</h1>
    </div>
  </header>

  <main>
    <section class="info">
      <h2 style="text-align: center">Select burger</h2>
      <p style="text-align: center">This is where you execute your burger selection</p> <!-- yallah -->
      <div class="wrapper">
        <Burger class="burgers" v-for="burger in burgers"
                v-bind:burger="burger"
                v-bind:key="burger.name"
                v-on:orderedBurger="addtoOrder($event)"/>

      </div>
    </section>

    <section style="clear: left" class="customer">
        <h1>Customer information</h1>
        <p1>Provide necessary information:</p1>

      <form><br>
        <p>Pinpoint your deleivery location</p><br>
        <div id="mapcontainer">
          <div id="map" v-on:click="setLocation">

            <div v-bind:style="{left: location.x + 'px', top: location.y + 'px'}">
              T
            </div>
          </div>
        </div>

      <p>
        <label for="Fullname">Fullname</label><br>
        <input type="text" id="Fullname" v-model="fn" required="required" placeholder="First and lastname">
      </p>
      <p>
        <label for="E-mail">E-mail</label><br>
        <input type="text" id="E-mail" v-model="em" placeholder="E-mail adress">
      </p>


      <p>
        <label for="payment">Payment</label>
        <select id="payment" v-model="pay"><br>
          <option>Credit card</option>
          <option>Diamonds</option>
          <option>Gold</option>
          <option>Swish</option>
        </select>
      </p>

        <p>Gender:</p>
        <div>
          <input type="radio" id="Male" v-model="gender" value="Male">
          <label for="Male">Male</label><br>

          <input type="radio" id="Female" v-model="gender" value="Female">
          <label for="Female">Female</label><br>

          <input type="radio" id="Other" v-model="gender" value="Other">
          <label for="Other">Other</label>
        </div>
        <br>
      </form>

      <button type="submit" v-on:click=sendOrder>Place my order</button>

    </section>
  </main>

</template>

<script>
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'
import menu from "/Users/filipf/WebbUtv/1md031-lab-21/src/assets/menu.json"


const socket = io();



/*
function MenuItem(name, kcal, pic, gluten, lactose) {
  this.name = name;
  this.kCal = kcal;
  this.picture = pic;
  this.gluten = gluten;
  this.lactose = lactose;
}
const BurgerArray= [new MenuItem("Jappaleno",800,"https://537924-1719237-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2021/02/Classic-homemade-hamburger-1024x683.jpg",true,true),
  new MenuItem("Meastro spezial",700,"https://silveroak.com/wp-content/uploads/2020/03/Recipe-Wagyu-Burger.jpg",true,false),
  new MenuItem("Vegan Burger",600,"https://cdn.vox-cdn.com/thumbor/G37rJxsEExFQgCybvOP44Ozg0Vk=/0x1155:2253x2845/1200x800/filters:focal(0x1155:2253x2845)/cdn.vox-cdn.com/uploads/chorus_image/image/48511073/151201_mr1cx_rugby_burger37888.0.0.jpg",false,false)
];
console.log(BurgerArray);

 */
const BurgerArray = menu


export default {
  name: 'Home',
  components: {
    Burger
  },

  data: function () {
    return {
      burgers: BurgerArray,
      pay: "",
      fn: "",
      em: "",
      gender: "",
      orderedBurger: {},
      location: { x: 0, y: 0},
      orderNum: 1
    }
  },

  methods: {
    getOrderNumber: function () {
      this.orderNum
      return this.orderNum++;
    },
    /*

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
       */

    sendOrder: function () {
      console.log(

          "Full name:", this.fn,
          "Email:", this.em,
          "Payment method:", this.pay,
          "Gender:", this.gender,
          "Ordered burgers:", this.orderedBurger);

      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: {
          fn: this.fn,
          em: this.em,
          pay: this.pay,
          gender: this.gender,
          x: this.location.x,
          y: this.location.y,
        },
        orderItems: [this.orderedBurger]

      })
    },

    addtoOrder: function (event) {
      this.orderedBurger[event.name] = event.amount;
    },
    setLocation: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top
      };
      this.location = {
        x: event.clientX - 10 - offset.x,
        y: event.clientY - 10 - offset.y
      }
    }
  }
}

</script>

<style>

body {
  font-family: "Times New Roman";
  background-color: #262424;
  font-size: 110%;
  border: Double 30px;
  border-color:dimgrey;
}


#container{
  width:95%;
  height:auto;
  opacity:90%;
  margin-top: 40px;

}
#headertext{
  position: absolute;
  margin-left: 480px;
  margin-top:-520px;
  font-size: 250%;
  color: #211e1e;
}
#headerimage{
  border:ridge 2px;
  border-radius: 20px;
  margin-top: -20px;
  margin-left: 30px;

}

.info{
  background-color:#262424;
  overflow: auto;
  color: antiquewhite;
  font-size: 120%;
}
.wrapper {
  display: grid;
  grid-gap: 160px;
  grid-template-columns: 300px 300px 300px;
  background-color:#262424;
  border: ridge 3px;
  border-radius: 20px;
  padding: 30px;
  margin:40px;
}
.customer{
  margin: 10px;
  background-color: #262424;
  color: antiquewhite;
  overflow: auto;
}

input[type="radio"] {
  height: .7em;
  width: .7em;
}
button:hover {
  cursor: pointer;
  background-color: lightgreen;

}
#headerimage{
  width: 100%;
  opacity: 0.95;
  overflow:hidden
}
#mapcontainer{
  width: 1200px;
  height: 500px;
  overflow:scroll;
  margin-bottom: 20px;
  border: ridge 3px;
  border-radius: 20px;
}
#map {
  position: relative;
  margin: 0;
  padding: 0;
  background: url(/img/polacks.jpg);
  background-repeat: no-repeat;
  width:1920px;
  height: 1078px;
  cursor: crosshair;
}
#map div {
  position: absolute;
  background: #f10808;
  color: #f10808;
  border-radius: 15px;
  width: 10px;
  height: 10px;
  text-align: center;
}
</style>