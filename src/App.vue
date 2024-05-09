<template>
  <div id="app">
    <h1>Pengeluaran Pribadi</h1>
    
    <form @submit.prevent="addExpense" class="expense-form">
      <div class="form-group">
        <label for="description" class="label-description">Nama Pembayaran :</label>
        <input type="text" id="description" v-model="newExpense.description" required>
      </div>

      <div class="form-group">
        <label for="amount" class="label-amount">Harga :</label>
        <div class="input-group">
          <span class="currency">Rp</span>
          <input type="number" id="amount" v-model.number="newExpense.amount" required>
        </div>
      </div>

      <button type="submit">Tambahkan</button>
    </form>

    <div class="expenses">
      <h2>Daftar Pengeluaran</h2>
      <ul> 
        <transition-group name="fade">
          <li v-for="(expense, index) in expenses" :key="index" :class="{ 'highlight': expense.amount > limit }">
            <span class="description">{{ expense.description }}</span> 
            <span class="amount">{{ expense.amount !== '' ? formatRupiah(expense.amount) : 'Mystery Price' }}</span>
            <button @click="removeExpense(index)" class="delete-button">Hapus</button>
          </li>
        </transition-group>
      </ul> 
      <div>Total Pengeluaran: {{ formatRupiah(totalAmount) }}</div>
    </div>
  </div>
</template>




<script>
export default {
  name: 'App',
  data() {
    return {
      newExpense: {
        description: '',
        amount: ''
      },
      expenses: [],
      limit: 50000,
      totalAmount: 0 
    };
  },
  methods: {
    addExpense() {
      this.expenses.push({...this.newExpense});
      this.newExpense.description = '';
      this.newExpense.amount = '';
      this.calculateTotal(); 
    },
    removeExpense(index) {
      this.expenses.splice(index, 1);
      this.calculateTotal(); 
    },
    formatRupiah(amount) {
      return new Intl.NumberFormat('id-ID', { style: 'currency', currency: 'IDR' }).format(amount);
    },
    calculateTotal() {
      this.totalAmount = this.expenses.reduce((acc, expense) => acc + parseFloat(expense.amount), 0);
    }
  }
};
</script>

<style scoped>

</style>

<style scoped>
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  background-color: #6ccb39;
}

#app {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  background-color: #56f4bf;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  color: #000000;
  margin-bottom: 30px;
}

.expense-form {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 15px;
}

.label-description {
  font-weight: bold;
  color: #1436e0; 
}

.label-amount {
  font-weight: bold;
  color: #1436e0;
}

.input-group {
  display: flex;
  align-items: center;
}

.currency {
  padding: 10px;
  background-color: #007bff;
  color: #23f92a;
  border-top-left-radius: 5px;
  border-bottom-left-radius: 5px;
}

input[type="text"],
input[type="number"] {
  width: calc(100% - 40px);
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #bdfc1d;
}

button[type="submit"] {
  background-color: #007bff;
  color: #23f92a; 
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
}

button[type="submit"]:hover {
  background-color: #0056b3;
}

.expenses {
  background-color: #4198ee;
  border-radius: 10px;
  padding: 20px;
}

.expenses h2 {
  font-size: 24px;
  color: #23f92a;
  margin-bottom: 20px;
}

.expenses ul {
  list-style: none;
  padding: 0;
}

.expenses li {
  border-bottom: 1px solid #53cce8;
  padding: 10px 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.expenses li:last-child {
  border-bottom: none;
}

.expenses .highlight {
  color: rgba(255, 0, 0, 0.015);
}

.expenses .description {
  flex: 1;
}

.delete-button {
  background-color: #ff4500;
  color: #ffff00;
  border: none;
  padding: 8px 15px;
  border-radius: 5px;
  cursor: pointer;
}

.delete-button:hover {
  background-color: #d60000;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
