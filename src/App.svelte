<script>
	import { quote } from './quotes';
	import Tailwind from './Components/Tailwind.svelte';

	let q = quote();
	let input = '';
	let timer = 0;
	$: start = false;

	function changeQuote(i) {
		if (q.quote === i) {
			q = quote();
			input = '';
			timer = 0;
		}
	}

	$: changeQuote(input);

	$: toggleStart = () => (start = !start);

	let startGame = () => {
		toggleStart();
		let ticker = setInterval(() => {
			if (start) {
				timer++;
			} else {
				clearInterval(ticker);
			}
		}, 1000);
	};
</script>

<Tailwind />
<main
	class="h-screen flex flex-col items-stretch p-2 bg-indigo-400 overflow-auto"
>
	<div
		class="text-gray-800 bg-gray-200 font-bold flex justify-center items-center m-2 header"
	>
		<div>Time: {timer}</div>
	</div>
	<div
		class=" h-auto w-full m-auto flex flex-col justify-between items-center text-center border-2 border-gray-800"
	>
		<div class="m-5">
			<div class="text-center">
				{#each q.quote as letter, index (index)}
					<span
						class:bg-green-500={input[index] === letter}
						class:bg-red-500={input[index] && input[index] !== letter}
						>{letter}</span
					>
				{/each}
			</div>
		</div>
		<div class="m-5 w-2/3 h-1/4">
			<textarea
				bind:value={input}
				type="text"
				class="h-full w-full text-xs border border-gray-800"
			/>
		</div>
	</div>
	<div class="text-red-500 text-center italic text-sm">{q.author}</div>

	<div class="h-1/5 flex justify-center space-x-2 items-center mb-2">
		<button on:click={() => startGame()} class="bg-red-500"
			>{!start ? 'Start' : 'Pause'}</button
		>
	</div>
</main>
