<template>
  <div class="component-container">
      <h2>Pagos</h2>
      <div class="transaction-cards-container">
          <h3 class="transaction-cards-container" v-if="transactions.length === 0">Sin pagos recibidos</h3>
          <div class="transaction-card" v-for="(transaction, index) in transactions" :key="index">
              <div class="transaction-content">
                  <p class="transaction-info"><strong>Total de propinas:</strong> {{ transaction.amount }}</p>
                  <p class="transaction-info"><strong>Método de Pago:</strong> {{ transaction.paymentMethod }}</p>
                  <p v-if="transaction.employeeCount" class="transaction-info"><strong>Total pagado:</strong> {{ transaction.employeeCount }} </p>
                  <button @click="removeTransaction(index)">Eliminar</button>
              </div>
          </div>
      </div>
      
      <button class="pay-tip-button" @click="addPayment">Pagar Propinas</button>
      <div class="payment-info">
          <p class="payment-info-item orange-text">Total Pagado: {{ totalPaid }} </p>
          <p class="payment-info-item ">Restante por pagar : {{ remainingToPay }} </p>
          
      </div>

  </div>
</template>

<script>
export default {
  data() {
      return {
          transactions: [],
          selectedMethod: '',
          tipPerEmployee: 0,
          totalTips: 0,
          totalPaid: 0,
      };
  },
  computed: {
    remainingToPay() {
      return (this.totalTips - this.totalPaid).toFixed(2);
    }
  },
  methods: {
      addTransaction(amount, paymentMethod, employeeCount) {
          this.transactions.unshift({ amount, paymentMethod, employeeCount });
          this.totalPaid += employeeCount; 
          localStorage.setItem('transactions', JSON.stringify(this.transactions));
          localStorage.setItem('totalPaid', this.totalPaid); 
      },
      removeTransaction(index) {
          const employeeCount = this.transactions[index].employeeCount;
          this.transactions.splice(index, 1);
          this.totalPaid -= employeeCount; 
          localStorage.setItem('transactions', JSON.stringify(this.transactions));
          localStorage.setItem('totalPaid', this.totalPaid); 
      },
      addPayment() {
          const selectedMethod = localStorage.getItem('selectedMethod');
          const tipPerEmployee = parseFloat(localStorage.getItem('tipPerEmployee'));
          const totalTips = parseFloat(localStorage.getItem('totalTips'));
          const newTransaction = { amount: totalTips, paymentMethod: selectedMethod, employeeCount: tipPerEmployee };
          this.addTransaction(newTransaction.amount, newTransaction.paymentMethod, newTransaction.employeeCount);
      }
  },
  created() {
    const storedTransactions = localStorage.getItem('transactions');
    if (storedTransactions) {
        this.transactions = JSON.parse(storedTransactions);
    } else {
        
        this.transactions.push({ amount: 20, paymentMethod: 'Efectivo', employeeCount: 1 });
        this.transactions.push({ amount: 30, paymentMethod: 'Tarjeta', employeeCount: 2 });
    }

    this.selectedMethod = localStorage.getItem('selectedMethod') || '';
    this.tipPerEmployee = parseFloat(localStorage.getItem('tipPerEmployee')) || 0;
    this.totalTips = parseFloat(localStorage.getItem('totalTips')) || 0;
    this.totalPaid = parseFloat(localStorage.getItem('totalPaid')) || 0; 
}
};
</script>

<style scoped>
.component-container {
  margin-top: -400px; 
  margin-left: 60%; 
}

ul {
  list-style-type: none;
  padding: 0;
}

.transaction-cards-container {
  max-height: 200px; 
  overflow-y: auto; 
}

.transaction-cards-container {
  max-width: 500px;
  max-height: 300px;
  overflow: auto;
}

.transaction-card {
  width: calc(70% - 10%); 
  margin-bottom: 20px;
  border: 1px solid #ccc;
  padding: 9px;
  border-radius: 30px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1), 0 1px 3px rgba(0, 0, 0, 0.08);
}

.transaction-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.transaction-info {
  margin: 5px 0; 
}

.pay-tip-button {
  width: 350px;
  border: 1px solid #ccc;
  padding: 15px 30px;
  cursor: pointer;
  background-color: #ffffff; 
  border-radius: 50px;
  box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
  margin-top: 90px; 
  margin-left: auto; 
}

.pay-tip-button:hover {
  background-color: #a29595; 
}

.payment-info {
  display: flex; 
  flex-direction: column; 
  align-items: flex-start;
  width: 100%;
  margin-top: -40px; 
  margin-left: -130%;
}

.payment-info-item {
  margin: 0;
  font-size: 1.2em; 
}

.orange-text {
  color: #FF6600;
}
</style>

