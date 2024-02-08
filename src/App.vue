<template>
  <div id="app">
    <TotalTips />
    <TipDivision />
    <PaymentMethods />
    <TransactionHistory :transactions="transactions" />
  </div>
</template>

<script>
import TipDivision from './components/TipDivision.vue';
import PaymentMethods from './components/PaymentMethods.vue';
import TransactionHistory from './components/TransactionHistory.vue';

export default {
  components: {

    TipDivision,
    PaymentMethods,
    TransactionHistory,
  },
  data() {
    return {
      transactions: []
    };
  },
  methods: {
    payTip() {
      try {
  
        this.transactions.unshift({ amount: this.totalTips, paymentMethod: this.selectedMethod, employeeCount: this.tipPerEmployee });
        console.log('Transacción agregada:', { amount: this.totalTips, paymentMethod: this.selectedMethod, employeeCount: this.tipPerEmployee });
      } catch (error) {
        console.error('Error al agregar la transacción:', error);
      }
    },
    handlePaymentMethodUpdate(paymentMethod) {
      try {
        this.transactions.unshift({ amount: this.totalTips, paymentMethod, employeeCount: null });
        console.log('Transacción agregada:', { amount: this.totalTips, paymentMethod, employeeCount: null });
      } catch (error) {
        console.error('Error al actualizar el método de pago:', error);
      }
    },
    handleTipPerEmployeeCalculated(tipPerEmployee) {
      try {
        this.transactions.unshift({ amount: this.totalTips, paymentMethod: 'cash', employeeCount: tipPerEmployee });
        console.log('Transacción agregada:', { amount: this.totalTips, paymentMethod: 'cash', employeeCount: tipPerEmployee });
      } catch (error) {
        console.error('Error al calcular propinas por empleado:', error);
      }
    }
  },
  created() {
    this.$root.$on('payment-method-updated', this.handlePaymentMethodUpdate);
    this.$root.$on('tip-per-employee-calculated', this.handleTipPerEmployeeCalculated);
  },
  beforeDestroy() {
    
    this.$root.$off('payment-method-updated', this.handlePaymentMethodUpdate);
    this.$root.$off('tip-per-employee-calculated', this.handleTipPerEmployeeCalculated);
  }
};
</script>

