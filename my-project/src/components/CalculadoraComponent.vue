<template>
    <div class = "hello">
      <h1>{{ msg }}</h1>
      <table class = "table table-bordered">
        <tbody>
          <tr>
            <td colspan="4" class="resultado">{{ output || 0 }}</td>
          </tr>
          <tr>
            <td v-on:click="clearField()">C</td>
            <td>+/-</td>
            <td v-on:click="calculaPorcentagem()">%</td>
            <td class="right-column" v-on:click="processOutput('divide')">/</td>
          </tr>

          <tr>
            <td v-on:click="getNumber('7')">7</td>
            <td v-on:click="getNumber('8')">8</td>
            <td v-on:click="getNumber('9')">9</td>
            <td class="right-column" v-on:click="processOutput('subtract')">-</td>
          </tr>
          <tr>
            <td v-on:click="getNumber('4')">4</td>
            <td v-on:click="getNumber('5')">5</td>
            <td v-on:click="getNumber('6')">6</td>
            <td class="right-column" v-on:click="processOutput('add')">+</td>
          </tr>
          <tr>
            <td v-on:click="getNumber('1')">1</td>
            <td v-on:click="getNumber('2')">2</td>
            <td v-on:click="getNumber('3')">3</td>
            <td class="right-column" v-on:click="processOutput('multiply')">x</td>
          </tr>
          <tr>
            <td colspan="2" v-on:click="getNumber('0')">0</td>
            <td v-on:click="getDot()">.</td>
            <td class="right-column" v-on:click="updateOutput()">=</td>
          </tr>
        </tbody>
      </table>
    </div>
</template>

<script>
export default {
  name: 'CalculadoraComponent',
  props: {
    msg: String
  },
  data () {
    return {
      output: '',
      previousValue: null,
      operationFired: false
    }
  },
  methods:{
    clearField(){
      this.output = '0';
    },
    setNegativeOrPositive(){
      this.output = this.output[0] === '-'? this.output.slice(1):`-${this.output}`;
    },
    calculatePercentage(){
      this.output = parseFloat(this.output)/100
    },
    getNumber(number){
      if(this.operationFired){
        this.output = '';
        this.operationFired=false;
      }
      this.output=`${this.output}${number}`
    },
    getDot(){
      if(this.output.indexOf('.') === -1){
        this.output = this.output+'.'
      }
    },
    processOutput(operation){

      if(operation==='add'){
        this.operation=(a, b)=>{
          return parseFloat(a)+parseFloat(b);
        }
      }
      else if(operation==='subtract'){
        this.operation=(a, b)=>{
          return parseFloat(a)-parseFloat(b);
        }
      }
      if(operation==='divide'){
        this.operation=(a, b)=>{
          return parseFloat(a)/parseFloat(b);
        }
      }
      if(operation==='multiply'){
        this.operation=(a, b)=>{
          return parseFloat(a)*parseFloat(b);
        }
      }
      this.previousValue = this.output;
      this.operationFired=true;
    },
    updateOutput(){
      this.output= `${this.operation(this.previousValue, this.output)}`;
      this.previousValue=null;
    }

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

table
{
  border: 1px solid rgba(0, 0, 0, 0.349);
}

.right-column {
  background-color: orange;
  color: black;
}

.right-column:hover
{
  background-color: rgba(255, 166, 0, 0.747);
}

td:hover
{
  background-color: rgba(128, 128, 128, 0.39);
}

.resultado
{ 
  color: white;
  font-weight: bold;
  background-color: #333;
  text-align: right;
}

.resultado:hover
{
  background-color: #333;
}

table
{
  width: 400px;
  margin: 0 auto;
  height: 400px;

  background-color: rgba(128, 128, 128, 0.336);

  font-weight: bold;
  font-size: 24px;
}

tr
{
  width: 10px;
}



</style>
