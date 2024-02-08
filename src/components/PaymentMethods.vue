<template>
<div>
    <label>Método de Pago:</label>
    <select v-model="selectedMethod" @change="updatePaymentMethod">
        <option value="cash">Efectivo</option>
        <option value="card">Tarjeta</option>
        <!-- Agrega más opciones si es necesario -->
    </select>
    </div>
  </template>
  
  <script>
  export default {
data() {
    return {
        selectedMethod: localStorage.getItem('selectedMethod') || 'cash'
}
    },
    methods: {
updatePaymentMethod() {
        localStorage.setItem('selectedMethod', this.selectedMethod);
        this.$root.$emit('payment-method-updated', this.selectedMethod); // Emitir evento cuando se seleccione un método de pago
    }
    }
}
</script>
