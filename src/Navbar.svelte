<script>
	export let isFormOpen;
</script>

<nav class="nav">
	<div class="nav-center">
		<h1 class="nav-title">Budget Calculator</h1>
		<button type="button" class="nav-btn" on:click={() => (isFormOpen = true)}>
			<i class="far fa-plus-square" />
			add item
		</button>
	</div>
</nav>
