<template>
    <Header />
    <div class="container">
        <Balance :total="+total" />
        <IncomeExpenses :income="+income" :expenses="+expenses" />
        <TransactionList @transactionDeleted="handleTransactionDeleted" :transactions="transactions" />
        <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
    </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

import { ref, computed, onMounted } from "vue";
import { useToast } from 'vue-toastification';

const toast = useToast();

const transactions = ref([]);


onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
    if (savedTransactions) {
        transactions.value = savedTransactions;
    }
});


// Get total
const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
        return acc + transaction.amount;
    }, 0);
});

// Get Income
const income = computed(() => {
    return transactions.value.filter((transaction) => transaction.amount > 0).reduce((acc, transaction) => {
        return acc + transaction.amount;
    }, 0);
});

// Get Expenses
const expenses = computed(() => {
    return transactions.value.filter((transaction) => transaction.amount < 0).reduce((acc, transaction) => {
        return acc + transaction.amount;
    }, 0);
});

// Add Transaction
const handleTransactionSubmitted = (transactionData) => {
    transactions.value.push({
        id: generateUniqueId(),
        title: transactionData.title,
        amount: transactionData.amount
    });
    savedTransactionsToLocalStorage();
    toast.success('Transaction added!');
};

// Generate unique ID
const generateUniqueId = () => {
    return Math.floor(Math.random() * 1000000);
};

// Delete Transaction
const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter((transaction) => transaction.id !== id);
    savedTransactionsToLocalStorage();
    toast.success("Transaction deleted!");
};

// Save to localstorage
const savedTransactionsToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value));
}
</script>