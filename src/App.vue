<!--================================ HTML Part=========================== -->
<template>
  <div id="app">
    <div class="container">
      <table>
        <tr>
          <td colspan="5">
            <div id="screen">
              <span id="screen_top">M=<span v-text="memory"></span></span>
              <div id="screen_bottom">
                <!-- v-text is a directive that is used to replace the content of HTML tag with private data -->
                <!-- It will update the content automatically when data is changed. It is called data reactive -->
                <span v-text="inputNumber" id="operand1" class="display">0</span>
                <!-- <span v-text="inputNumber" id="operand1">0</span> -->
                <span id="operator"></span>
                <span id="operand2"></span> 
              </div>
              <!-- <span id="screen_bottom">0</span> -->
            </div>
          </td>
        </tr>
        <tr>
          <td>
            <button type="button" class="btn btn-warning" @click="mc()">MC</button>
          </td>
          <td>
            <button type="button" class="btn btn-warning" @click="mr()">MR</button>
          </td>
          <td>
            <button type="button" class="btn btn-warning" @click="mMinus()">M-</button>
          </td>
          <td>
            <button type="button" class="btn btn-warning" @click="mPlus()">M+</button>
          </td>
          <td>
            <button type="button" class="btn btn-light">
              <i class="fa fa-long-arrow-right" aria-hidden="true"></i>
            </button>
          </td>
        </tr>
        <tr>
          <td>
            <button type="button" class="btn btn-light" v-on:click="append(7)">7</button>
          </td>
          <td>
            <button type="button" class="btn btn-light" v-on:click="append(8)">8</button>
          </td>
          <td>
            <button type="button" class="btn btn-light" v-on:click="append(9)">9</button>
          </td>
          <td>
            <button type="button" class="btn btn-secondary" @click="devide()">÷</button>
          </td>
          <td>
            <button type="button" class="btn btn-light" @click="min()">+/-</button>
          </td>
        </tr>
        <tr>
          <td>
            <button type="button" class="btn btn-light" v-on:click="append(4)">4</button>
          </td>
          <td>
            <button type="button" class="btn btn-light" v-on:click="append(5)">5</button>
          </td>
          <td>
            <button type="button" class="btn btn-light" v-on:click="append(6)">6</button>
          </td>
          <td>
            <button type="button" class="btn btn-secondary" v-on:click="mul()">x</button>
          </td>
          <td>
            <button type="button" class="btn btn-secondary" @click="minus()" >-</button>
          </td>
        </tr>
        <tr>
          <td>
            <button
            v-on:click="append(1)"
              type="button"
              class="btn btn-light"
            >
              1
            </button>
          </td>
          <td>
            <button type="button" class="btn btn-light" v-on:click="append(2)">2</button>
          </td>
          <td>
            <button type="button" class="btn btn-light" v-on:click="append(3)">3</button>
          </td>
          <td rowspan="2">
            <button type="button" class="btn btn-secondary long-btn" @click="add()">+</button>
          </td>
          <td rowspan="2">
            <button type="button" class="btn btn-primary long-btn" @click="equal()">=</button>
          </td>
        </tr>
        <tr>
          <td>
            <button type="button" class="btn btn-danger" @click="clear()">C</button>
          </td>
          <td>
            <button type="button" class="btn btn-light" v-on:click="append(0)">0</button>
          </td>
          <td>
            <button type="button" class="btn btn-light" @click="dot()">.</button>
          </td>
        </tr>
      </table>
    </div>
    <div class="alert alert-danger" id="message_panel" role="alert">
      something wrong here
    </div>
  </div>
</template>
<!--===================================javascript part================================== -->
<script>
// import { operation } from 'retry';

// import { equal } from 'assert';
// import { multiply } from 'lodash';

export default {
  name: 'App',
  components: {},
  data() {
    return {
      // This is the private data section which can be used inside this component
      inputNumber: '',
      operator:null,

      lastInput:null,
      temporary:null,
      operatorClicked:false,
      memory:0,
      
    
    };
  },
  methods: {
    showNumber(number) {
      // Assign number when user click to the inputNumber data
      // To access private data from methods, use (this.)
      this.inputNumber = number;
     
    },
    // changeOperation(opcode){
    //   this.operation = opcode;

    // },
    append(number){
      if(this.operatorClicked){
        this.inputNumber='';
        this.operatorClicked=false;
      }
      // if(this.inputNumber === '0'){
      //   this.inputNumber = '';

      // }else{
      //   this.inputNumber = `${this.inputNumber}${number}`;
      // }
      this.inputNumber = `${this.inputNumber}${number}`;

  },
  dot(){
    if(this.inputNumber.indexOf('.')<=0){// click dot only 1 time 
      this.append('.');

    }
  },
  clear(){
    this.inputNumber=' ';
  },
  min(){
    if(this.inputNumber.charAt(0)==='-'){
      this.inputNumber=this.inputNumber.slice(1);
    }else{
      this.inputNumber=`-${this.inputNumber}`;
    }
  
   
  },
  add(){
    this.operator =(a,b)=>a+b;
    this.lastInput=this.inputNumber;
    this.operatorClicked = true;
  },
  mul(){
    this.operator =(a,b)=>a*b;
    this.lastInput=this.inputNumber;
    this.operatorClicked = true;
  },
  devide(){
    this.operator =(a,b)=>a/b;
    this.lastInput=this.inputNumber;
    this.operatorClicked = true;
  },
  minus(){
    this.operator =(a,b)=> -(a-b);
    this.lastInput=this.inputNumber;
    this.operatorClicked = true;
  },
  equal(){
    this.temporary = this.inputNumber;
    this.inputNumber= `${this.operator(parseFloat(this.temporary),parseFloat(this.lastInput))}`;
    this.lastInput=null;
  },
  mc(){
    this.memory = 0;
  },
  mr(){
    var n = `${this.inputNumber}`;
    if(n[(n.length)-2] == "+" || n[(n.length)-2] == "-" || n[(n.length)-2] == "*" || n[(n.length)-2] == "/"){
        n += this.memory;
      }
  },
  mMinus(){
  
    this.memory = eval(this.memory + " - " + this.inputNumber);
  },
  mPlus(){
    this.memory = eval(this.memory + " + " + this.inputNumber);
  

  }
  
   
  },


};
</script>
<!--=================== CSS Part===================== -->
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.container {
  margin-top: 10em;
  width: 300px;
  border: 1px solid black;
  padding-top: 20px;
  padding-bottom: 20px;
}
table {
  border-spacing: 7px;
  border-collapse: separate;
}
#screen {
  border: 1px solid black;
  padding: 7px;
  width: 100%;
  height: 4em;
}
#screen_top {
  display: block;
  font-size: 0.8rem;
}
#screen_bottom {
  font-size: 1.8rem;
  display: block;
  text-align: right;
}
#operand2 {
  background-color: skyblue;
}
#operator {
  background-color: rosybrown;
}
.button-row {
  display: flex;
  justify-content: space-between;
}
button {
  width: 45px;
}
.long-btn {
  display: inline-block;
  height: 80px;
}

/* Message panel */
#message_panel {
  width: 300px;
  margin-top: 1em;
  display: none;
  margin-left: auto;
  margin-right: auto;
}
</style>
