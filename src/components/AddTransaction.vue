<template>
    <h3>Add new transaction</h3>
    <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
            <label for="text">Text</label>
            <input type="text" v-model="title" id="text" placeholder="Enter text..." />
        </div>
        <div class="form-control">
            <label for="amount">Amount <br />
                (negative - expense, positive - income)</label>
            <input type="number" v-model="amount" id="amount" placeholder="Enter amount..." />
        </div>
        <button class="btn">Add transaction</button>
    </form>
</template>

<script setup>
import { ref } from "vue";
import { useToast } from 'vue-toastification';

const title = ref('');
const amount = ref('');

const emit = defineEmits(['transactionSubmitted']);
const toast = useToast();

const onSubmit = () => {
    if (!title.value || !amount.value) {
        toast.error("Both fields must be filled");
        return;
    }
    const transactionData = {
        title: title.value,
        amount: parseInt(amount.value)
    }

    console.log(transactionData);

    emit('transactionSubmitted', transactionData);

    title.value = '';
    amount.value = '';
}
</script>