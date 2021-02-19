<script>
	// Imports - Svelte
	import { setContext, onMount, afterUpdate } from 'svelte';

	// Imports - Components
	import ExpenseForm from './Components/expenses/ExpenseForm.svelte';
	import Navbar from './Components/app-wide/Navbar.svelte';
	import ExpenseList from './Components/expenses/ExpenseList.svelte';
	import Totals from './Components/app-wide/Totals.svelte';
	import Modal from './Components/app-wide/Modal.svelte';

	//Variables
	let expenses = [];
	let isFormOpen = false;
	let setName = '';
	let setAmount = '';
	let setId = '';

	//Reactive - accumulator and current
	$: total = expenses.reduce((acc, expense) => {
		return (acc += expense.amount);
	}, 0);
	$: isEditing = setId ? true : false;

	// Functions
	function removeExpense(id) {
		expenses = expenses.filter((item) => {
			return item.id !== id;
		});
	}

	function clearExpenses() {
		expenses = [];
	}

	function addExpense({ name, amount }) {
		let expense = { id: Math.random() * Date.now(), name, amount };
		expenses = [...expenses, expense];
	}

	function setEditVariables(id) {
		// set variables
		let expense = expenses.find((item) => id === item.id);
		setId = expense.id;
		setName = expense.name;
		setAmount = expense.amount;
		isFormOpen = true;
	}

	function editExpense({ name, amount }) {
		expenses = expenses.map((item) => {
			console.log(item.id);
			return item.id === setId ? { ...item, name, amount } : { ...item };
		});
		setId = null;
		setName = '';
		setAmount = null;
	}

	// Context
	setContext('remove', removeExpense);
	setContext('edit', setEditVariables);

	// Local Storage
	function setLocalStorage() {
		localStorage.setItem('expenses', JSON.stringify(expenses));
	}

	// Lifecycle
	onMount(() => {
		expenses = localStorage.getItem('expenses')
			? JSON.parse(localStorage.getItem('expenses'))
			: [];
	});
	afterUpdate(() => {
		setLocalStorage();
	});
</script>

<Modal bind:isFormOpen>
	<ExpenseForm
		formTitle={isEditing ? 'edit' : 'add'}
		name={setName}
		amount={setAmount}
		{addExpense}
		{editExpense}
		bind:isFormOpen
	/>
</Modal>
<Navbar bind:isFormOpen />
<main class="content">
	<Totals title="Total Expenses" {total} />
	<ExpenseList {expenses} />
	{#if expenses.length > 0}
		<button
			type="button"
			class="btn btn-primary btn-block"
			on:click={clearExpenses}>CLEAR EXPENSES</button
		>
	{/if}
</main>
