<script>
	// Imports - Componets
	import Title from './Title.svelte';

	// Props
	export let name;
	export let amount;
	export let addExpense;
	export let editExpense;
	export let isFormOpen;
	export let formTitle;

	// Reactive
	$: isEmpty = !name || !amount;

	// Functions
	function handleSubmit() {
		formTitle.toLowerCase() === 'add'
			? addExpense({ name, amount })
			: editExpense({ name, amount });
		name = '';
		amount = null;
	}
</script>

{#if isFormOpen}
	<section class="form">
		<Title
			>{formTitle.charAt(0).toUpperCase() + formTitle.slice(1)} Expense</Title
		>
		<form on:submit|preventDefault={handleSubmit} class="expense-form">
			<div class="form-control">
				<label for="name">Name</label>
				<input type="text" id="name" bind:value={name} />
			</div>
			<div class="form-control">
				<label for="amount">Amount</label>
				<input type="number" id="amount" bind:value={amount} />
			</div>
			{#if isEmpty}
				<p class="form-empty">Please fill out all form fileds</p>
			{/if}
			<button type="submit" class:disabled={isEmpty} class="btn btn-block"
				>{formTitle} EXPENSE</button
			>
			<button
				type="button"
				class="close-btn"
				on:click={() => (isFormOpen = false)}
				><i class="fas fa-times" /> Close</button
			>
		</form>
	</section>
{/if}
