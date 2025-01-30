<script lang="ts">
	import '@fontsource/ultra';
	import '@fontsource/cambay';
	import '@fontsource/sanchez';
	import '../app.css';
</script>

<div class="background"></div>

<div class="container">
	<slot />
</div>

<footer>
    <a href="https://github.com/LeoDog896/guess-the-color/">GitHub</a>.
    Made with Malice and Hatred.
</footer>

<style>
	div.background {
		width: 100vw;
		height: 100vh;
		position: fixed;
		background-image: repeating-conic-gradient(#ffc15e 0 9deg, #f7b05b 9deg 18deg);
		transition: opacity 1s cubic-bezier(0.075, 0.82, 0.165, 1);
		opacity: 1;
		user-select: none;
		z-index: -1;
	}

	:global(body.game) div.background {
		opacity: 0;
	}

	div.container {
		display: flex;
		width: 100%;
		height: 100%;
		align-items: center;
		justify-content: center;
	}

    footer {
        position: fixed;
        bottom: 0;
        width: 100%;
        padding: 0.5rem;
        border-top: 2px solid black;
        background-color: white;
        text-align: center;
    }
</style>
