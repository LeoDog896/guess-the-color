<script lang="ts">
	import Button from '$lib/Button.svelte';

	interface Props {
		transition(): void;
	}

	let { transition }: Props = $props();
</script>

<main>
	<h1>GUESS THE COLOR</h1>

	<h2>Get a color. Guess the name.</h2>

	<Button onclick={transition}>Play</Button>
</main>

<style>
	main {
		text-align: center;
	}

	@media (max-width: 600px) {
		h1 {
			font-size: 4rem;
		}
	}
</style>
