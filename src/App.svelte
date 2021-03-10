<script>
	import { quote } from './quotes';
	import Tailwind from './Components/Tailwind.svelte';

	let q = quote();
	let input = '';
	let timer = 0;
	let elm;

	$: start = false;

	function changeQuote(i) {
		if (q.quote === i) {
			q = quote();
			input = '';
			start = false;
			start = true;
			timer = 0;
		}
	}

	$: changeQuote(input);

	$: toggleStart = () => (start = !start);

	const startGame = () => {
		toggleStart();
		if (start) {
			elm.focus();
		}
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
	class="h-screen flex flex-col items-center p-2 bg-gray-900 overflow-auto mx-w-1/2"
>
	<div class="h-full w-full sm:w-2/4 sm:border sm:border-indigo-800 rounded-md">
		<div
			class="text-gray-800 bg-gray-400 rounded-md font-bold border border-indigo-800 flex justify-center items-center m-2 header"
		>
			<div>Time: {timer}</div>
		</div>
		<div
			class="h-5/6 w-full m-auto flex flex-col justify-evenly items-center text-center rounded-md"
		>
			<div class="h-2/5 mx-5">
				<div class="text-white">
					{#each q.quote as letter, index (index)}
						<span
							class:bg-green-500={input[index] === letter}
							class:bg-red-500={input[index] && input[index] !== letter}
							>{letter}</span
						>
					{/each}
				</div>
			</div>
			<div class=" w-2/3 h-1/4">
				<textarea
					bind:value={input}
					type="text"
					bind:this={elm}
					class="h-5/6 w-full text-xs border bg-gray-400 border-indigo-800 rounded-md outline-none"
				/>
				<div
					class="flex justify-center space-x-2 items-center outline-none mb-2"
				>
					<button
						on:click={() => startGame()}
						class="bg-red-500 border border-indigo-800 shadow-md rounded-md outline-none"
					>
						{!start ? 'Start' : 'Pause'}
					</button>
				</div>
			</div>
		</div>
		<div class="text-red-500 text-center italic text-sm">{q.author}</div>
	</div>
</main>
