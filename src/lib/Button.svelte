<script lang="ts">
	import type { Snippet } from 'svelte';

	interface Props {
		onclick(): void;
		children?: Snippet;
		theme?: {
			top: string;
			bottom: string;
			color: string;
		};
	}

	let { onclick, children, theme }: Props = $props();
</script>

<button
	{onclick}
	style:--bottom={theme?.bottom ?? '#AD2E24'}
	style:--top={theme?.top ?? '#C75146'}
	style:color={theme?.color ?? 'black'}
>
	<span>
		{@render children?.()}
	</span>
</button>

<style>
	button {
		font-size: 2rem;
		padding: 0;
		border: none;
		background-color: var(--bottom);
		border-radius: 0.5rem;
		cursor: pointer;
	}

	button span {
		display: block;
		padding: 1rem 5rem;
		background-color: var(--top);
		border-radius: 0.5rem;
		transform: translateY(-4px);
		outline-offset: 4px;
		transition: transform 250ms;
	}

	button:hover span {
		transform: translateY(-6px);
	}

	button:active span {
		transform: translateY(-2px);
	}
</style>
