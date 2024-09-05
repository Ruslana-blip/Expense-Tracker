<template>
	<Header />
	<div class="container">
		<Balance :total="+total" />
		<IncomeExpenses :incame="+incame" :expenses="+expenses" />
		<TransactionList
			:transations="transations"
			@transactionDeleted="handleTransactionDeleted"
		/>
		<AddTransaction @transactionsSubmitted="handleTransactionsSubmitted" />
	</div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

import { ref, computed, onMounted } from 'vue';
import { useToast } from 'vue-toastification';

const toast = useToast();

const transations = ref([]);

onMounted(() => {
	const saveTransactions = JSON.parse(localStorage.getItem('transactions'));
	if (saveTransactions) {
		transations.value = saveTransactions;
	}
});

const total = computed(() => {
	return transations.value.reduce((acc, transation) => {
		return acc + transation.amount;
	}, 0);
});

const incame = computed(() => {
	return transations.value
		.filter(transation => transation.amount > 0)
		.reduce((acc, transation) => {
			return acc + transation.amount;
		}, 0)
		.toFixed(2);
});

const expenses = computed(() => {
	return transations.value
		.filter(transation => transation.amount < 0)
		.reduce((acc, transation) => {
			return acc + transation.amount;
		}, 0)
		.toFixed(2);
});

const handleTransactionsSubmitted = transactionData => {
	transations.value.push({
		id: generateUniqueId(),
		text: transactionData.text,
		amount: transactionData.amount,
	});
	saveTransactionsToLocalStorage();

	toast.success('Transaction added');
};
const generateUniqueId = () => {
	return Math.floor(Math.random() * 100000);
};

const handleTransactionDeleted = id => {
	transations.value = transations.value.filter(
		transaction => transaction.id !== id
	);

	saveTransactionsToLocalStorage();

	toast.success('Transaction deleted');
};

const saveTransactionsToLocalStorage = () => {
	localStorage.setItem('transactions', JSON.stringify(transations.value));
};
</script>

<style></style>
