<template>
<div>

  <div class="payment-info">
    <p class="payment-info-item  blak-text-Titilo">Pagos de propinas </p>
    <p class="payment-info-item orange-text-Titilo ">Efectivo en caja: 5,500 </p>
  </div>
  <div class="input-container">
    <div class="contenedor-imput">

      <div class="input-fields">
        <label for="totalTips" class="total-price-label">Monto Total de Propinas:</label>            
        <div class="input-field">
          <input type="text" id="totalTips" v-model="totalTipsValue" @focus="activeField = 'totalTips'">
        </div>
        <label for="numEmployees">¿Entre cúantos quieres dividir las Propinas?</label>
        <input class="imput-Empreado" type="text" id="numEmployees" v-model="numEmployeesValue" @focus="activeField = 'numEmployees'">
        <button class="calculate-button" @click="calculateTipPerEmployee">Calcular Propina</button>
        <p v-if="tipPerEmployee > 0">Propina por Empleado: {{ tipPerEmployee }} por empleado</p>
        <p v-else>No se ha calculado la propina por empleado.</p>
      </div>
    </div>
    
    <div class="calculator-container">
      <div class="numeric-keypad" v-show="showKeypad">
        <button v-for="num in [1,2,3,4,5,6,7,8,9,0]" :key="num" @click="appendNumber(num)" class="numeric-keypad-button">{{ num }}</button>
        <button @click="clearInput" class="numeric-keypad-button">Clear</button>
      </div>
    </div>
  </div>
  
</div>  


  </template>
  
  <script>
  export default {
    data() {
      return {
        isDivided: false,
        totalTipsValue: localStorage.getItem('totalTips') || 0,
        numEmployeesValue: '',
        tipPerEmployee: localStorage.getItem('tipPerEmployee') || 0,
        showKeypad: false, 
        activeField: null 
      }
    },
    methods: {
      calculateTipPerEmployee() {
        const totalTips = parseFloat(this.totalTipsValue);
        const numEmployees = parseFloat(this.numEmployeesValue);
  
        if (!isNaN(totalTips) && !isNaN(numEmployees) && totalTips > 0 && numEmployees > 0) {
          this.tipPerEmployee = (totalTips / numEmployees).toFixed(2);
          this.$root.$emit('tip-per-employee-calculated', this.tipPerEmployee);
          localStorage.setItem('tipPerEmployee', this.tipPerEmployee);
          localStorage.setItem('totalTips', this.totalTipsValue);
          this.totalTipsValue = ''; 
          this.numEmployeesValue = ''; 
        } else {
          this.tipPerEmployee = 0;
        }
      },
      appendNumber(num) {
        if (this.activeField === 'totalTips') {
          this.totalTipsValue += num;
        } else if (this.activeField === 'numEmployees') {
          this.numEmployeesValue += num;
        }
      },
      clearInput() {
        if (this.activeField === 'totalTips') {
          this.totalTipsValue = '';
        } else if (this.activeField === 'numEmployees') {
          this.numEmployeesValue = '';
        }
      },
      updateTotalTips() {
        localStorage.setItem('totalTips', this.totalTipsValue);
        this.$root.$emit('total-tips-updated', parseFloat(this.totalTipsValue));
      }
    },
    mounted() {
      this.showKeypad = true;
      this.$root.$on('total-tips-updated', totalTips => {
        this.totalTipsValue = totalTips.toString();
      });
    }
  }
  </script>
  
  <style>
  .input-container {
    display: flex;
    align-items: center;
    margin-bottom: 8px; 
  }

  .payment-info {
  width: 100%;
  display: flex;
  justify-content: space-between; 
  border-top: 1px solid #ccc; 
  margin-top: 10px; 
}

.payment-info-item {
  margin-left: 5%;
}
  
  .input-fields {
    display: flex;
    flex-direction: column;
    margin-right: 20px; 
  }
  
  .calculator-container {
    background-color: rgba(238, 211, 182, 0.711);
    padding: 15px;
    border-radius: 10px;
    width: 220px; 
    height: 250px;
    margin-left: 5% ;
    margin-top: 5px;
  }
  
  .input-field {
    position: relative;
    background-color: #FFDAB9; 
    color: #FF6600; 
    border: 1px solid orange;
    padding: 10px;
    margin: 5px;
    border-radius: 5px;
    width: 150px;
    height: 23px;
    
  }
  
  .input-field input {
    color: #FF6600; 
    font-size: 1.7em; 
    background-color: #FFDAB9; 
    border: none;
    width: 100%;
    text-align: center;
    outline: none;
    cursor: pointer;
    margin: 0;
    padding: 0;
    border-radius: 15px
  }
  
  .total-price {
    font-size: 1.7em; 
    color: #FF6600; 
    margin-bottom: 5px;
    margin-left: 10px;
  }
  
  .total-price-label {
    font-size: 0.6em; 
    color: #FF6600; 
    margin-bottom: 5px;
    margin-left: 10px;
  }
  
  .calculator-container input[type="text"],
  .calculator-container button {
    background-color: white;
    color: black;
    border: 1px solid rgb(131, 121, 101);
    padding: 10px;
    margin: 1px;
    border-radius: 5px;
  }
  
  .numeric-keypad {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 5px;
    margin-top: 10px;
  }
  
  .numeric-keypad-button {
    padding: 10px;
    font-size: 16px;
    background-color: orange;
    color: white;
    border: none;
    border-radius: 5px;
  }

  .contenedor-imput{
    margin-left: 5%;
    margin-top: -7%;
  }

  .orange-text-Titilo  {
  color: #FF6600;
  background-color: #ff66003c;
  font-size: 2rem;
}

.blak-text-Titilo{
  color: #000;
  font-size: 2rem;
}

.calculate-button{
  background-color: #FF6600;
  color: #fff;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 10px;
}
.imput-Empreado{
  width: 70px;
  height: 23px;
  border-radius: 10px;
}
  </style>
  