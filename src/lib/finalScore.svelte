<script>
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();

	import Reward from './Reward.svelte';
	import { score } from './Store.js';

	function reset() {
		score.set(0);
	}
</script>

<div class="finalpage">
	<div class="medal">
		<div class="bg-circle">
			<Reward />
		</div>
		<div class="reward-text">you won a medal. <br /> can play once more to win a trophy</div>
	</div>
	<div class="score-header">final Score</div>
	<div class="score-number">{$score}</div>
	<button
		on:click={() => {
			dispatch('restart', 'home'), reset();
		}}
		class="button"
	>
		Restart Game
	</button>
</div>

<style>
	.finalpage {
		height: 100%;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		font-family: var(--display);
		text-align: center;
	}

	.medal {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}

	.bg-circle {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 12rem;
		height: 12rem;
		border-radius: 100%;
		background-color: #fefbf5;
	}

	.reward-text {
		font-size: 1.5rem;
		line-height: 1.2;
	}

	.score-header {
		font-size: 2rem;
		margin-top: 4rem;
	}

	.score-number {
		font-size: 5.5rem;
		margin-top: -2rem;
	}

	.button {
		background-color: #d96f23;
		font-size: 1.75rem;
		color: var(--white);
	}

	@media (width < 600px) {
		.medal {
			margin-top: -6rem;
		}
		.reward-text {
			font-size: 1.2rem;
		}
		.score-header {
			font-size: 1.5rem;
			margin-top: 3rem;
		}
		.score-number {
			font-size: 4.5rem;
		}
		.button {
			font-size: 1.2rem;
		}
	}
</style>
