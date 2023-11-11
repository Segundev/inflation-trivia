<script>
	import data from '$lib/data.json';
	import Coin from './Coin.svelte';
	import Question from './Question.svelte';
	import { score } from './Store.js';
	import FinalScore from './finalScore.svelte';

	import { fly } from 'svelte/transition';
	import { quintOut } from 'svelte/easing';

	let questionsArray;
	let questionCount = 0;
	let choiceClicked, endGame;

	function getData(dataArr) {
		const randomItems = dataArr.slice();
		for (let i = randomItems.length - 1; i > 0; i--) {
			const j = Math.floor(Math.random() * (i + 1));
			[randomItems[i], randomItems[j]] = [randomItems[j], randomItems[i]];
		}
		return randomItems.slice(0, 10);
	}

	questionsArray = getData(data);

	$: if (questionCount === 10) endGame = true;
</script>

<div
	in:fly={{ x: -1000, duration: 1500, easing: quintOut, delay: 250 }}
	class="gamescreen-wrapper"
	class:choiceClicked
>
	<div class="top-nav-layout">
		<div class="score-count">
			<Coin />
			<div class="progress-wrapper">
				<div class="progress-bar" />
			</div>
			<div class="score" class:choiceClicked>
				{$score}
			</div>
		</div>
	</div>
	<div class="screen">
		{#each questionsArray as question, index}
			{#if questionCount === index}
				<Question
					{question}
					{index}
					on:nextQuestion={(e) => {
						(questionCount = e.detail), (choiceClicked = false);
					}}
					on:optionSelected={(e) => (choiceClicked = true)}
				/>
			{/if}
		{/each}
		{#if endGame}
			<FinalScore on:restart />
		{/if}
	</div>
</div>

<style>
	.gamescreen-wrapper {
		height: 100vh;
		position: relative;
	}

	.gamescreen-wrapper.choiceClicked {
		background-image: url('/images/market.jpg');
	}

	.top-nav-layout {
		position: fixed;
		width: 100%;
		padding-top: 1.5rem;
	}

	.screen {
		padding-top: 3rem;
		width: 100%;
		height: 100%;
	}

	.top-nav-layout .score-count {
		float: right;
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
		position: relative;
		margin-right: 50px;
	}

	.score-count .progress-wrapper {
		width: 120px;
		height: 20px;
		background-color: #f4f4f4;
		border-radius: 6rem;
		margin-left: -10px;
		display: flex;
		flex-direction: column;
	}

	.progress-bar {
		width: 80%;
		height: 100%;
		margin-top: 2.5px;
		margin-bottom: 2.5px;
		padding-right: 1px;
		background-color: var(--yellow);
		border-radius: 30px;
	}

	.score {
		padding-left: 10px;
		font-size: 2rem;
		font-family: var(--display);
	}

	.score.choiceClicked {
		color: var(--white);
	}
</style>
