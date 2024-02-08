
<template>
  <div id="app">
    <TotalTips />
    <TipDivision />
    <PaymentMethods />
    <TransactionHistory :transactions="transactions" />
  </div>
</template>

<script>
// import TotalTips from './components/TotalTips.vue';
import TipDivision from './components/TipDivision.vue';
import PaymentMethods from './components/PaymentMethods.vue';
import TransactionHistory from './components/TransactionHistory.vue';

export default {
  components: {
    // TotalTips,
    TipDivision,
    PaymentMethods,
    TransactionHistory
  },
  data() {
    return {
      transactions: []
    }
  },
  methods: {
    payTip() {
      // Agregar una nueva transacción con el monto total de propinas y el método de pago seleccionado
      this.transactions.unshift({ amount: this.totalTips, paymentMethod: this.selectedMethod, employeeCount: this.tipPerEmployee });
      console.log('Transacción agregada: aaaaaa', { amount: this.totalTips, paymentMethod: this.selectedMethod, employeeCount: this.tipPerEmployee });
    }
  },
    created() {
      this.$root.$on('payment-method-updated', paymentMethod => {
        this.transactions.unshift({ amount: this.totalTips, paymentMethod, employeeCount: null });
        console.log('Transacción agregada create:', { amount: this.totalTips, paymentMethod, employeeCount: null });
      });
      
      this.$root.$on('tip-per-employee-calculated', tipPerEmployee => {
        this.transactions.unshift({ amount: this.totalTips, paymentMethod: 'cash', employeeCount: tipPerEmployee });
        console.log('Transacción agregada: va', { amount: this.totalTips, paymentMethod: 'cash', employeeCount: tipPerEmployee });
      });
    }
  }
</script>
