<template>
  <div>
    <h3 class="burgertext" v-bind:key=burger.name> {{ burger.name }}</h3>
    <img class="burgerpic" v-bind:src="burger.img" alt="Burger" style="width: 300px;height:200px">
    <ul class="list">
      <li v-bind:key=burger.kCal> <span>{{ burger.kCal }}</span> Calories</li>
      <li v-if="burger.lactose">Contains <span class="lactose">Lactose</span></li>
      <li v-else>Does not contain <span class="lactose">Lactose</span></li>
      <li v-if="burger.gluten">Contains <span class="gluten">Gluten</span></li>
      <li v-else>Does not contain <span class="gluten">Gluten</span></li>

      <button type="button" v-on:click="removeBurger">
        -
      </button>

      <button type="button" v-on:click="addBurger">
        +
      </button>

      <p> Shopping Cart: {{ amountOrdered }} </p>

    </ul>
  </div>
</template>

<script>
export default {
  name: 'Burger',
  props: {
    burger: Object
  },
  data: function () {
    return {
      amountOrdered: 0,
    }
  },
  methods: {
    removeBurger: function () {
      this.amountOrdered--
      this.$emit('orderedBurger', {
            name: this.burger.name,
            amount: this.amountOrdered}
      );
    },
    addBurger: function () {
      this.amountOrdered++
      this.$emit('orderedBurger', {
            name: this.burger.name,
            amount: this.amountOrdered
          }
      );
    }
  }
}


</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.gluten {
  font-weight: bold;
  color: #a40e0e;
}
.lactose {
  font-weight: bold;
  color: #a40e0e;
}
.list{
  font-size: 90%;
}

.burgerpic{
  border: ridge 3px;
  border-radius: 20px;
}
.burgertext{
  text-align: center;
}

</style>
