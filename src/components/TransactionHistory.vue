
<template>
    <div>
    <button @click="addPayment">Pagar Propina</button>
    <h2>Historial de Transacciones</h2>
    <ul>
        <li v-for="(transaction, index) in transactions" :key="index">
        <p><strong>Total de propinas:</strong> {{ transaction.amount }}</p>
        <p><strong>Método de Pago:</strong> {{ transaction.paymentMethod }}</p>
        <p v-if="transaction.employeeCount"><strong>Total pagado:</strong> {{ transaction.employeeCount }} </p>
        <button @click="removeTransaction(index)">Eliminar</button>
    </li>
    </ul>
    </div>
</template>

<script>
export default {
data() {
    return {
        transactions: [],
        selectedMethod: '',
        tipPerEmployee: 0,
        totalTips: 0
      };
    },
    methods: {
    addTransaction(amount, paymentMethod, employeeCount) {
        this.transactions.unshift({ amount, paymentMethod, employeeCount });
        localStorage.setItem('transactions', JSON.stringify(this.transactions));
},
    removeTransaction(index) {
        this.transactions.splice(index, 1);
        localStorage.setItem('transactions', JSON.stringify(this.transactions));
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
        // Simular algunas transacciones de ejemplo si no hay ninguna almacenada
}


    this.selectedMethod = localStorage.getItem('selectedMethod') || '';
    this.tipPerEmployee = parseFloat(localStorage.getItem('tipPerEmployee')) || 0;
    this.totalTips = parseFloat(localStorage.getItem('totalTips')) || 0;
    }
};
</script>

<style scoped>

ul {
    list-style-type: none;
    padding: 0;
}
li {
    margin-bottom: 20px;
    border: 1px solid #ccc;
    padding: 10px;
}
</style>
