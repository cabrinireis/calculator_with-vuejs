<template>
<div class="container_calc">
  <div class="camada_a">
    <div class="logo" style="color: white;">
    <!-- <picture class="logo"> <img logo__image src="../assets/" alt=""></picture> -->
      <h1>Calculadora</h1>
    </div>
    <div class="camada_b">
      <div class="displ" >
        <div class="prev">{{current}}</div>
        <div  class="prev">{{result}}</div>
      </div>
      <div class="camada_c">
        <div class="grid-container">
          <div 
            v-for="item in buttons" :key="item.id"
            class="btn"
            :class="{ btn_op: item.type ==='operator'}"
            @click="buttonClick(item)"
          >
            <div v-if="item.type !== 'backspace' " :class="item.class" >
              {{item.value }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
</template>

<script>
export default {

  name: 'CalculadoraEmix',
  data() {
    return {
      botao:false,
      buttons: [
        {
          type: 'operator',
          value: 'C',
          class: 'reset'
        },
        {
          type: 'operator',
          value:  '',
          class: 'clear'
        },
        {
          type: 'operator',
          value: '',
          class: 'raiz'
        },
        {
          type: 'operator',
          value: '/',
          class: 'divide'
        },
         {
          type: 'number',
          value: '7',
           class: 'seven'
        },
        {
          type: 'number',
          value: '8',
           class: 'eight'
        },
        {
          type: 'number',
          value: '9',
           class: 'nine'
        },
        {
          type: 'operator',
          value: '*',
          class: 'multiply'
        },
        {
          type: 'number',
          value: '4',
           class: 'four',
        },
        {
          type: 'number',
          value: '5',
           class: 'five'
        },
        {
          type: 'number',
          value: '6',
           class: 'six'
        },
        {
          type: 'operator',
          value: '-',
          class: 'decrease'
        },
        {
          type: 'number',
          value: 1,
          class: 'one'
        },
        {
          type: 'number',
          value: '2',
           class: 'two'
        },
        {
          type: 'number',
          value: '3',
           class: 'three'
        },
        {
          type: 'operator',
          value: '+',
          class: 'add'
        },
        {
          type: 'ps_operator',
          value: '+/-',
          class: 'bigger_smaller'
        },
        {
          type: 'number',
          value: '0',
           class: 'zero'
        },
        {
          type: 'number',
          value: '.',
          class: 'pont'
        },
        {
          type: 'operator',
          value: '=',
          class: 'equal'
        },
        
        
       
        
      ],
      keyenter:{
        value:'',
        type:'',
        class:''
      },
      preview: [],
      operations: {
        '+': (a , b ) => a + b,
        '-': (a , b ) => a - b,
        '/': (a , b ) => a / b,
        '*': (a , b ) => a * b
      },
      constante: false,
      current: "",
      currentValue: "",
      currentOperation: '',
      result: "",
      keyboard: ''
    }
  },
  methods: {
    reset(){
        this.current = ''
        this.result = ''
        this.preview = []
        this.constante = false
        this.currentOperation = ""
    },
    buttonClick(item) {
      if (item.type === 'number'){
        if(this.constante == true){
          this.reset()
        }else if(this.currentValue == null ){
          this.currentValue += Number(item.value);
          this.result = ''
        }
          this.result += item.value;
      }
      
      else if(item.class === 'reset'){
        this.reset();
      }
      
      else if(item.class === 'clear') {
        this.result = this.result.slice(0, 0-1)
      }
      
      else if(item.class === 'equal' && this.currentValue != null){
        if(this.constante == true ){
          this.current = this.result + this.currentOperation + this.currentValue + item.value
          this.result = this.operations[this.currentOperation](this.result, this.currentValue)
        }else{
          this.preview.push( Number(this.result) )
          this.current += this.result + item.value;
          if(this.currentOperation === "/" && this.currentValue === 0) {
            this.result = "Não é possível dividir por zero";
            this.constante = true
          }else{
            this.result = this.operations[this.currentOperation](this.preview[0], +this.result)
            this.constante = true
          }
        }
      }
      else if(item.value == "+/-" && this.result !== ""){
        this.result *=  -1
      }
      else if(item.class === "raiz" ){
          if(item.value){
            this.currentValue = null;
            this.result *= 0 ;
          }else{
            this.constante = false
            this.current = item.value
            this.result = Math.sqrt(this.result) 
          }
            this.currentValue = null;
        
      }
      
      else if(item.type === 'operator' && this.currentValue != null ){
        this.currentValue = null;
        this.preview.push( Number(this.result) )
        this.current += this.result + item.value;
        if(this.preview.length >= 2){
          this.result = this.operations[this.currentOperation](this.preview[0], +this.result)
          this.preview = this.preview.slice(1,2);
          this.preview.unshift(this.result)
        }
          this.currentOperation = item.value
      }

      
        
    }
  },
  mounted () {
    
    window.addEventListener('keyup',(e)=>{
      if(e.key.match(/^[\d + \- . * / =]$/))
      {
        if(e.key === '+'){
          this.keyenter=[]
          this.keyenter.type = 'operator';
          this.keyenter.value = e.key;
          return this.buttonClick(this.keyenter);
        }
        if(e.key === '-'){
          this.keyenter=[]
          this.keyenter.type = 'operator';
          this.keyenter.value = '-';
          return this.buttonClick(this.keyenter);
        }
        if(e.key === '*'){
          this.keyenter=[]
          this.keyenter.type = 'operator';
          this.keyenter.value = e.key;
          return this.buttonClick(this.keyenter);
        }
        if(e.key === '/'){
          this.keyenter=[]
          this.keyenter.type = 'operator';
          this.keyenter.value = e.key;
          return this.buttonClick(this.keyenter);
        }
        if(e.key === '='){
          this.keyenter=[]
          this.keyenter.type = 'operator';
          this.keyenter.value = e.key;
          this.keyenter.class = "equal"
          return this.buttonClick(this.keyenter);
        }
        this.keyenter.type = 'number';
        this.keyenter.value = e.key;
       return this.buttonClick(this.keyenter);
      }
      else if(e.key === 'Enter'){
          this.keyenter.type = 'operator';
          this.keyenter.value = '=';
          this.keyenter.class = "equal"
          return this.buttonClick(this.keyenter);
        }
      else if(e.key === 'Delete'){
          this.keyenter=[]
          this.keyenter.class = "reset"
          return this.buttonClick(this.keyenter);
        }
      else if(e.key === 'Backspace'){
          // this.keyenter.type = 'operator';
          this.keyenter=[]
          this.keyenter.class = "clear"
          return this.buttonClick(this.keyenter);
        }
    });
  },
  props: {
    msg: String
  }
}
</script>


<style lang="scss" >

  @import 'style.scss';

</style>
