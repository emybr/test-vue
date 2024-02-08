<template>
    <div class="contenedor-pago" >
      <label for="paymentMethod">Método de Pago:</label>
      <div class="payment-method-buttons">
        <button :class="{ 'selected': selectedMethod === 'cash' }" @click="selectPaymentMethod('cash')">Efectivo</button>
        <button :class="{ 'selected': selectedMethod === 'BBVA' }" @click="selectPaymentMethod('BBVA')">BBVA</button>
        <button :class="{ 'selected': selectedMethod === 'Santander' }" @click="selectPaymentMethod('Santander')">Santander</button>
      </div>
      
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        selectedMethod: localStorage.getItem('selectedMethod') || 'cash'
      };
    },
    methods: {
      selectPaymentMethod(method) {
        this.selectedMethod = method;
        localStorage.setItem('selectedMethod', method);
        this.$root.$emit('payment-method-updated', method);
      }
    }
  };
  </script>
  
  <style>
  .payment-method-buttons {
    display: flex;
  }
  
  .payment-method-buttons button {
    border: 1px solid #ccc;
    padding: 5px 10px;
    margin-right: 10px;
    cursor: pointer;
    background-color: white; 
    width: 90px;
    height: 40px;
    border-radius: 10px;
    box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.7);
  }
  
  .payment-method-buttons button.selected {
    border-color: rgb(233, 112, 41);
  }
  
  .payment-method-label {
    margin-right: 10%;
  }

  .contenedor-pago {
    margin-left: 5%;
    margin-top: 40px;
  }
  </style>
  
  
