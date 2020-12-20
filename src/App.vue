<template>
  <div v-if="menu === 'startUp'">
    <h1>meny</h1>
    <button @click="register()">Go and register members</button>
    box would be nice place
    <!-- menu button -->
  </div>

  <div v-if="menu === 'register'">
    <h1>registering</h1>

    <button>next</button>

    <button @click="done()">done</button>

    <!-- show total people so far -->
    <!-- put your name here -->
    <!-- done button  -->
  </div>

  <div v-if="menu === 'reveal'">
    <!-- reveal function -->
    <!-- alert say them name hey are you NOzomu ? make sure no one's around -->
    <h1>
      <!-- should I send the data to google? -->
      are you ready?
    </h1>



  </div> 
  
</template>

<script>


export default {
  name: 'App',
  data(){
    return{
      menu: 'startUp',

    }
  },
  methods:{
    register(){
      this.menu = 'register'
    },
    done(){
      this.menu = 'reveal'
    },


  },


  
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
