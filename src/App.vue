<script setup>
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpenses from './components/IncomeExpenses.vue';
  import AddTransaction from './components/AddTransaction.vue';
  import {ref, computed} from 'vue'
  
  const transactions = ref([])


// const transactions = ref([
    // {id: 1, text: 'Paycheck', amount: 699.99},
    // {id: 2,   text: 'Food', amount: -30},
    // {id: 3, text: 'Bills', amount: -200},
    // {id: 4, text: 'Video Game', amount: -54.11},
  //])

  //get the total
  const total = computed(() => {
    return transactions.value.reduce( (acc, transaction) => {
      return acc + transaction.amount
    }, 0)
  })
//getting the income by adding the positive values
  const income = computed(()  => {
    return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce( (acc, transaction) => {
      return acc + transaction.amount
    }, 0) 
  })
// to get the expense by adding the negative values
  const expense = computed(()  => {
    return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce( (acc, transaction) => {
      return acc + transaction.amount
    }, 0) 
  })

  //handle transaction submitted
  const handleTransactionSubmitted = (transactionData) => {
    transactions.value.push({
      id: generateUniqueId(),
      text: transactionData.text,
      amount: transactionData.amount,
    })
  }

  //generate unique id
  const generateUniqueId = () => {
    return Math.floor(Math.random() * 10000000)
  }


</script>

<template>
  <Header></Header>
  <div  class="container">
    <Balance :total="total"></Balance>
    <IncomeExpenses :income="income" :expense="expense"></IncomeExpenses>
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted"></AddTransaction>
    <!-- {{ transactions }}  -->
  </div>
</template>