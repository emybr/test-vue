
   <template>
    <div class="">
      <div class="input-fields">
        <div class="imput-field">
            <label for="totalTips">Monto Total de Propinas:</label>            
            <input type="text" id="totalTips" v-model="totalTipsValue" @focus="activeField = 'totalTips'">
        </div>   
    <div>
            <div>    
                <label>¿Entre cuantos empleados quiere dividir la propina Dividir Propinas entre Empleados:?</label>
            </div>
            <label for="numEmployees">Número de Empleados:</label>
            <input type="text" id="numEmployees" v-model="numEmployeesValue" @focus="activeField = 'numEmployees'">
              <div v-if="tipPerEmployee > 0">
                <button @click="calculateTipPerEmployee">Calcular</button>
            <p>Propina por Empleado: {{ tipPerEmployee }} por empleado</p>
          </div>
          <div v-else>
            <p>No se ha calculado la propina por empleado.</p>
          </div>
        </div>
      </div>
      <!-- Teclado numérico -->
      <div class="numeric-keypad" v-show="showKeypad">
        <button v-for="num in [1,2,3,4,5,6,7,8,9,0]" :key="num" @click="appendNumber(num)" class="numeric-keypad-button">{{ num }}</button>
        <button @click="clearInput" class="numeric-keypad-button">Clear</button>
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
        showKeypad: false, // Inicialmente oculto
        activeField: null // Campo de entrada activo
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
          this.totalTipsValue = ''; // Reiniciar el campo después del cálculo
          this.numEmployeesValue = ''; // Reiniciar el campo después del cálculo
        } else {
          this.tipPerEmployee = 0;
        }
      },
      appendNumber(num) {
        // Agrega el número seleccionado al campo de entrada activo
        if (this.activeField === 'totalTips') {
          this.totalTipsValue += num;
        } else if (this.activeField === 'numEmployees') {
          this.numEmployeesValue += num;
        }
      },
      clearInput() {
        // Limpia el campo de entrada activo
        if (this.activeField === 'totalTips') {
          this.totalTipsValue = '';
        } else if (this.activeField === 'numEmployees') {
          this.numEmployeesValue = '';
        }
      },
      updateTotalTips() {
        // Actualiza el valor de totalTips y emite un evento cuando cambie
        localStorage.setItem('totalTips', this.totalTipsValue);
        this.$root.$emit('total-tips-updated', parseFloat(this.totalTipsValue));
      }
    },
    mounted() {
      // Mostrar el teclado numérico al principio
      this.showKeypad = true;
      this.$root.$on('total-tips-updated', totalTips => {
        this.totalTipsValue = totalTips.toString();
      });
    }
  }
  </script>
  
  <style>
  .calculator-container {
    background-color: orange;
    padding: 20px;
    border-radius: 10px;
    width: 300px; /* Ajusta el ancho de la calculadora según sea necesario */
  }
  
  .input-fields {
    display: flex;
    flex-direction: column;
  }
  
  .input-fields > div {
    margin-top: 10px;
  }
  
  .calculator-container input[type="text"],
  .calculator-container button {
    background-color: white;
    color: black;
    border: 1px solid orange;
    padding: 10px;
    margin: 5px;
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

  .imput-field {
    position: relative;
    background-color: #FFDAB9; 
    color: #FF6600; 
    border: 1px solid orange;
    padding: 10px;
    margin: 5px;
    border-radius: 5px;
}

  </style>
  