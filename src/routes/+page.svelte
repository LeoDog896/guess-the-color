<script lang="ts">
	import Homepage from "./Homepage.svelte";
	import Game from "./Game.svelte";

    let state = $state<'home' | 'game'>('home')

    $effect(() => {
        if (state === 'game') {
            document.body.classList.add('game')
        }
    })
</script>

{#if state === 'home'}
    <Homepage transition={() => state = 'game'}/>
{:else if state === 'game'}
    <Game />
{/if}
