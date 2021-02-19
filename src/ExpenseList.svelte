<script>
	import Title from './Title.svelte';
	import Expense from './SingleExpense.svelte';
	export let expenses = [];
</script>

<section>
	<Title>Expense list</Title>

	<ul>
		{#each expenses as expense, index}
			<Expense {...expense} />
		{:else}
			<h1>You, currently have no expenses</h1>
		{/each}
	</ul>
</section>
