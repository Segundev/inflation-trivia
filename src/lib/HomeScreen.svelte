<script>
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();
</script>

<div class="homescreen-wrapper">
	<header>
		<h1>Surviving Inflation</h1>
		<p>
			This Quiz helps you to understand the impact of inflation on everyday life and learn how to
			cope in a changing economic landscape.
		</p>
	</header>
	<!-- svelte-ignore a11y-no-static-element-interactions -->
	<!-- svelte-ignore a11y-click-events-have-key-events -->
	<div
		class="button-wrapper button"
		on:click={() => {
			dispatch('clickNext', 'game');
		}}
	>
		<div>Play the game</div>
		<svg viewBox="0 0 24 24"><title>Next button</title><path d="M10,17L15,12L10,7V17Z" /></svg>
	</div>
</div>

<style>
	.homescreen-wrapper {
		background-color: var(--yellow);
		height: 100%;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		gap: 11rem;
	}

	header {
		text-align: center;
		width: 600px;
		margin: 0 auto;
	}

	header h1 {
		font-family: var(--display);
		font-size: 3.5rem;
		word-wrap: break-word;
		color: var(--white);
		margin-bottom: 1rem;
	}

	header p {
		font-size: 1.2rem;
		color: var(--black);
	}

	.button-wrapper div {
		font-size: 1.2rem;
		color: var(--black);
	}

	.button-wrapper svg {
		height: 2rem;
		width: 2rem;
		fill: var(--black);
	}

	.button-wrapper {
		display: flex;
		align-items: center;
		justify-content: center;
		font-family: var(--display);
		background-color: var(--green);
		padding: 0.5rem 1.5rem;
		margin-top: 1rem;
		animation-name: nextButton;
		animation-duration: 1s;
		animation-timing-function: cubic-bezier(0.01, 0.5, 0.5, 1);
	}

	@keyframes nextButton {
		0% {
			transform: translateY(50px);
			opacity: 0.6;
		}

		to {
			transform: 0;
			opacity: 1;
		}
	}

	.button-wrapper:hover {
		background-color: var(--blue);
		border-color: unset;
		color: white;
		cursor: pointer;
		transition-property: background-color;
		transition-duration: 1.5s;
	}

	.button:focus,
	.button:visited,
	.button:active {
		outline: none;
	}

	@media (width < 600px) {
		header {
			width: 375px;
		}

		header h1 {
			font-size: 3rem;
		}

		header p {
			font-size: 1rem;
			line-height: 1.45;
		}
	}

	@media (width < 400px) {
		header {
			width: 350px;
		}
	}
</style>
