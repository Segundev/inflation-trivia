<script>
	import { fly, fade } from 'svelte/transition';
	import { quadOut } from 'svelte/easing';

	import { score } from './Store.js';
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();

	export let question;
	export let index;
	let isChoosed = false;
	let isOpen = true;

	let optionSelect;
	let selectedAnswer;

	function increment() {
		score.update((n) => n + 1);
	}

	$: if (selectedAnswer === question.response) increment();
</script>

<div class="gamescreen question" class:isChoosed>
	<div class="question-wrapper">
		<svg
			class="question-icon"
			viewBox="0 0 36 36"
			xmlns="http://www.w3.org/2000/svg"
			xmlns:xlink="http://www.w3.org/1999/xlink"
			aria-hidden="true"
			preserveAspectRatio="xMidYMid meet"
			><path
				fill="#D93229"
				d="M17 27a3 3 0 0 1-3-3v-4a3 3 0 0 1 3-3c.603-.006 6-1 6-5c0-2-2-4-5-4c-2.441 0-4 2-4 3a3 3 0 1 1-6 0c0-4.878 4.58-9 10-9c8 0 11 5.982 11 11c0 4.145-2.277 7.313-6.413 8.92c-.9.351-1.79.587-2.587.747V24a3 3 0 0 1-3 3z"
			/><circle fill="#D93229" cx="17" cy="32" r="3" /></svg
		>
		<div class="header">Question {index + 1}</div>
		<div class="text">{question.question}</div>
	</div>
	<div class="button-select-wrapper">
		<button
			class="button"
			on:click={() => {
				(isChoosed = true), (isOpen = false), (selectedAnswer = true);
				dispatch('optionSelected', { response: question.response, answer: selectedAnswer });
			}}>true</button
		>
		<button
			class="button"
			on:click={() => {
				(isChoosed = true),
					(isOpen = false),
					(selectedAnswer = false),
					dispatch('optionSelected', { response: question.response, answer: selectedAnswer });
			}}>false</button
		>
	</div>
</div>

<div in:fade={{ y: 0, duration: 2500, easing: quadOut }} class="gamescreen answer" class:isOpen>
	<div class="header">Correct Answer: {question.response}</div>
	<div class="text">{question.answer}</div>
	<button
		class="button"
		on:click={() => {
			dispatch('nextQuestion', (index += 1));
		}}>{index === 9 ? 'See score' : 'Next'}</button
	>
</div>

<style>
	.gamescreen {
		display: flex;
		flex-direction: column;
		text-align: center;
		align-items: center;
		height: 100%;
		margin-top: 4rem;
	}

	.question-wrapper {
		position: relative;
		background-image: url('/images/paper-image-dk.png');
		padding: 2rem;
		height: 18rem;
	}

	.header {
		font-size: 1.75rem;
		font-family: var(--display);
		padding-bottom: 3rem;
	}

	.text {
		font-size: 1.2rem;
		inline-size: 485px;
		overflow-wrap: break-word;
	}

	.question-icon {
		position: absolute;
		width: 42px;
		height: 42px;
		top: -1.2rem;
		left: 1rem;
	}

	.button-select-wrapper {
		display: flex;
		flex-direction: row;
		gap: 2.5rem;
		margin-top: 4rem;
	}

	.button {
		background: var(--white);
		padding: 0.2rem 2.5rem;
		color: var(--black);
		border: 0.5px #c8c8c8 solid;
		box-shadow: 0px 4px 8px rgba(64, 64, 64, 0.24);
	}

	.isChoosed {
		display: none;
	}

	.isOpen {
		display: none;
	}

	.answer {
		margin-top: 13rem;
		color: var(--white);
	}

	.button:focus,
	.button:visited,
	.button:active {
		outline: none;
	}

	.answer .button {
		margin-top: 3rem;
		background-color: var(--yellow);
	}

	@media (width < 600px) {
		.text {
			inline-size: 360px;
		}

		.button {
			font-size: 1.5rem;
		}
	}

	@media (width < 400px) {
		.text {
			inline-size: 340px;
		}
	}
</style>
