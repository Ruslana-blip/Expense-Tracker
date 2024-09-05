<template>
	<div class="inc-exp-container">
		<div>
			<h4>Income</h4>
			<p id="money-plus" class="money plus">+${{ incame }}</p>
		</div>

		<div>
			<h4>Expense</h4>
			<p id="money-minus" class="money minus">${{ expenses }}</p>
		</div>
	</div>
</template>

<script setup>
import { defineProps } from 'vue';

const props = defineProps({
	incame: {
		type: Number,
		required: true,
	},
	expenses: {
		type: Number,
		required: true,
	},
});
</script>

<style></style>
