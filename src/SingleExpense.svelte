<script>
	// Imports - Svelte
	import { getContext } from 'svelte';
	import { slide, scale, fade } from 'svelte/transition';
	// props
	export let id;
	export let name = '';
	export let amount = 0;

	// variables
	let displayAmount = false;

	// functions
	function toggleDisplayAmount() {
		return (displayAmount = !displayAmount);
	}

	const removeExpense = getContext('remove');
	const editExpense = getContext('edit');
</script>

<article class="single-expense" in:scale out:fade>
	<div class="expense-info">
		<h2>
			{name}
			<button class="amount-btn"
				><i class="fas fa-caret-down" on:click={toggleDisplayAmount} /></button
			>
		</h2>
		{#if displayAmount}
			<h4 transition:slide>amount: ${amount}</h4>
		{/if}
	</div>
	<div class="expense-buttons">
		<button class="expense-btn edit-btn" on:click={() => editExpense(id)}>
			<i class="fas fa-pen" />
		</button>
		<button class="expense-btn delete-btn" on:click={() => removeExpense(id)}>
			<i class="fas fa-trash" />
		</button>
	</div>
</article>
