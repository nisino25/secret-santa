<template>
  <div v-if="menu === 'startUp'">
    <h1>Main menu</h1>
    <button @click="register()">Go and register members</button>
    <br>
    <!-- <button @click="practice()">Practice</button> -->
    <!-- box would be nice place -->
    <!-- menu button -->
  </div>

  <div v-if="menu === 'register'">
    <div v-if="typing">
      <h1>registering</h1>
      <input type="text" placeholder="Type your name here " v-model="username" @keyup.enter="next('register')">&nbsp;
      <button @click="next('register')" >register</button>
    </div>

    <div v-if="!typing">
      <button @click="next('more')">Add more</button>
      <button @click="next('done')">done</button>
    </div>

    <div v-if="nameList !== []">
      <h3>We have {{nameList.length}} people now</h3>
      <li v-for="(name, i) in nameList" :key="i">{{name.name}}</li>
    </div>
  </div>

  <div v-if="menu === 'price'">
    <input type="number" placeholder="Type the minimum price" v-model="minimum" @keyup.enter="next('price')">&nbsp;
    <button @click="next('price')" >Go with this price</button>

  </div>

  <div v-if="menu === 'reveal'">
    <button @click="next('individual')" v-if="this.revealCount <= this.nameList.length" >Click this if you are {{this.nameList[revealCount].name}} </button>
    <button @click="next('individual')"  v-else>Check everything </button>
  </div> 

  <div v-if="menu ==='showtime'">
    <strong v-if="this.revealCount <= this.nameList.length">{{this.nameList[revealCount].name}}, you are sending gift to {{this.nameList[revealCount].givingTo}} </strong><br>
    <button @click="next('back')" >Done</button>

    <div v-if="this.revealCount > this.nameList.length">
      <table>
        <tr>
          <th>Name&nbsp;&nbsp;</th>
          <th>Sending to &nbsp; &nbsp;</th>
          <th>Getting from </th>
        </tr>
        <tr v-for="(name, i) in nameList" :key="i">
          <td>{{name.name}}</td>
          <td>{{name.givingTo}}</td>
          <td>{{name.recievingFrom}}</td>
        </tr>
      </table>
      <br>
    </div>
    


  </div>
  
</template>

<script>


export default {
  name: 'App',
  data(){
    return{
      menu: 'startUp',
      username: '',
      nameList: [],
      confirmMsg: null,
      typing: true,
      listCount: 0,
      r: null,
      minimum: null,
      randomNum: null,
      name: [],
      checked: false,
      nameDict: {},
      secretMsg: null,
      confirm: false,
      revealCount: 0,
    }
  },


  methods:{
    next(condition){
      switch(condition){
        case 'more':
          this.typing = true
          return;
        
        case 'register':
          if(this.username === ''){
            alert('It cannot be a blank')
            return;
          }
          this.nameList.push( {
            name: this.username,
            givingTo: null,
            recievingFrom: null,
          });

          this.username= ''


          if(this.nameList.length > 5){
            this.typing = false;
          }
          return
          
        case 'done':
          this.listCount = 0;
          this.confirmMsg = ''
          while(this.listCount < this.nameList.length){

            this.confirmMsg = this.confirmMsg +  `${this.nameList[this.listCount].name}\r\n `

            this.listCount++
          }
            
          this.r= confirm(`We have ${this.nameList.length} people now \r\n \r\n ${this.confirmMsg}`);
            if(!this.r){
              return;
            }
          this.menu = 'price'
          return

        case 'price':
          if(this.minimum === null){
            alert('Put some number please')
            return;
          }
          this.r= confirm(`The minimum price is ${this.minimum}$`);
            if(!this.r){
              return;
            }
          this.menu = 'reveal'
          return;
         
        case 'individual':
          this.menu = 'showtime';

          if(this.revealCount ===0){
            this.assign();
          }


          return;

        case 'back':
          this.menu = 'reveal'
          this.revealCount++

      }
    },
    assign(){
      let secretCount = 0
      

      while(secretCount < this.nameList.length){
        this.checked = false;


        this.randomNum = Math.random();
        this.randomNum = parseInt(this.randomNum * (this.nameList.length))

        if(secretCount === this.randomNum){
          this.assign()
          return;

          // if(secretCount === this.nameList.length -1){
          //   // This would be an inifinite loop
          //   this.assign()
          //   return;
          // }
          // while(secretCount === this.randomNum){
          //   this.randomNum = Math.random();
          //   this.randomNum = parseInt(this.randomNum * (this.nameList.length))
          // }
        }

        
        this.check(this.randomNum)



        while(!this.checked){
          this.randomNum = Math.random();
          this.randomNum = parseInt(this.randomNum * (this.nameList.length))
          if(secretCount === this.randomNum){
            this.assign()
            return;
          }
          this.check(this.randomNum)
        }

        this.nameList[this.randomNum].recievingFrom = this.nameList[secretCount].name;
        this.nameList[secretCount].givingTo = this.nameList[this.randomNum].name;

        secretCount++;
      }
      this.menu = 'showtime';
      
      

    },
    check(num){
      if(this.nameList[num].recievingFrom === null){
        this.checked = true;
        return;
      }
      this.checked = false;
    },

    practice(){
    },

    register(){
      this.menu = 'register'
      this.typing = true;
    },
    

  },
  async mounted(){
    

  }

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

*{
  font-size: 115%;
}
</style>
