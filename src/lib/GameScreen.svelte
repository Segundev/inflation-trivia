<script>
	import data from '$lib/data.json';
	import Coin from './Coin.svelte';
	import Question from './Question.svelte';
	import { score } from './Store.js';
	import FinalScore from './finalScore.svelte';

	import { fly } from 'svelte/transition';
	import { quintOut } from 'svelte/easing';
	import { tweened } from 'svelte/motion';
	import Play from './Play.svelte';
	import Pause from './Pause.svelte';

	const tweenedScore = tweened(0);
	$: tweenedScore.set($score);

	let questionsArray;
	let questionCount = 0;
	let endGame;
	let isPlaying = false;
	let audio;
	let choiceClicked;
	let isRight;
	let isWrong;

	function getData(dataArr) {
		const randomItems = dataArr.slice();
		for (let i = randomItems.length - 1; i > 0; i--) {
			const j = Math.floor(Math.random() * (i + 1));
			[randomItems[i], randomItems[j]] = [randomItems[j], randomItems[i]];
		}
		return randomItems.slice(0, 10);
	}

	function toggleAudio() {
		if (isPlaying) {
			audio.pause();
		} else {
			audio.play();
			audio.volume = 0.01;
		}
		isPlaying = !isPlaying;
	}

	questionsArray = getData(data);

	$: if (questionCount === 10) endGame = true;

	$: if (choiceClicked) isRight = choiceClicked.response === choiceClicked.answer ? true : false;
	$: if (choiceClicked) isWrong = choiceClicked.response !== choiceClicked.answer ? true : false;
</script>

<div
	in:fly={{ x: -1000, duration: 1500, easing: quintOut, delay: 250 }}
	class="gamescreen-wrapper"
	class:isRight
	class:isWrong
>
	<div class="top-nav-layout">
		<div class="audio-wrapper">
			<audio id="background-music" loop bind:this={audio}>
				<source src="audio.mp3" type="audio/mpeg" />
				Your browser does not support the audio element.
			</audio>
			<div on:click={toggleAudio}>
				{#if isPlaying}
					<Play />
				{:else}
					<Pause />
				{/if}
			</div>
		</div>
		<div class="score-count">
			<Coin />
			<div class="progress-wrapper">
				<div class="progress-bar" style="width:{$tweenedScore * 10}%" />
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
						(questionCount = e.detail), (isRight = null), (isWrong = null);
					}}
					on:optionSelected={(e) => {
						choiceClicked = e.detail;
					}}
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

	.gamescreen-wrapper.isRight {
		/* background-image: url('/images/market.jpg'); */
		background-color: var(--green);
	}

	.gamescreen-wrapper.isWrong {
		/* background-image: url('/images/market.jpg'); */
		background-color: var(--red);
	}

	.top-nav-layout {
		position: fixed;
		width: 100%;
		margin-top: 1.5rem;
		padding: 0 1rem;
		display: flex;
		justify-content: space-between;
		align-items: center;
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
	}

	.score-count .progress-wrapper {
		width: 120px;
		height: 12px;
		background-color: #f4f4f4;
		border-radius: 6rem;
		margin-left: -10px;
		display: flex;
		flex-direction: column;
	}

	.progress-bar {
		height: 100%;
		margin-top: 2.5px;
		margin-bottom: 2.5px;
		margin-right: 2px;
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
