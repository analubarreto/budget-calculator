<script>
	import { fade } from 'svelte/transition';
	export let isFormOpen;
</script>

{#if isFormOpen}
	<div class="modal-container" transition:fade>
		<div class="modal-content">
			<slot />
		</div>
	</div>
{/if}
