<script lang="ts">
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();

	const color = ['#888888', '#fdfdfd'];
	export let key_colors: Array<number> = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0];
	export let training_board = true;

	function forwardBW(v: number) {
		dispatch('BW_input', {
			value: v
		});
	}
</script>

<div class="keypad">
	<button class="key digit" id="key1" style="background-color:{color[key_colors[1]]}">1</button>
	<button class="key digit" id="key2" style="background-color:{color[key_colors[2]]}">2</button>
	<button class="key digit" id="key3" style="background-color:{color[key_colors[3]]}">3</button>
	<button class="key digit" id="key4" style="background-color:{color[key_colors[4]]}">4</button>
	<button class="key digit" id="key5" style="background-color:{color[key_colors[5]]}">5</button>
	<button class="key digit" id="key6" style="background-color:{color[key_colors[6]]}">6</button>
	<button class="key digit" id="key7" style="background-color:{color[key_colors[7]]}">7</button>
	<button class="key digit" id="key8" style="background-color:{color[key_colors[8]]}">8</button>
	<button class="key digit" id="key9" style="background-color:{color[key_colors[9]]}">9</button>
	{#if training_board}
		<button
			class="key color_key"
			id="key_black"
			on:click={() => forwardBW(0)}
			style="background-color:{color[0]}">My Digit is Black</button
		>
	{:else}
		<button
			class="key color_key"
			id="key_black"
			on:click={() => forwardBW(0)}
			style="background-color:{color[0]}">+</button
		>
	{/if}
	<button class="key digit" id="key0" style="background-color:{color[key_colors[0]]}">0</button>
	{#if training_board}
		<button
			class="key color_key"
			id="key_white"
			on:click={() => forwardBW(1)}
			style="background-color:{color[1]}">My Digit is White</button
		>
	{:else}
		<button
			class="key color_key"
			id="key_white"
			on:click={() => forwardBW(1)}
			style="background-color:{color[1]}">+</button
		>
	{/if}
</div>

<style>
	.keypad {
		display: grid;
		grid-template-columns: repeat(3, 1fr);
		grid-auto-rows: minmax(100px, auto);
		min-width: 90vw;
		border-style: solid;
	}
	.key {
		display: flex;
		border-style: solid;
		border-color: black;
		background: white;
		align-items: center;
		justify-content: center;
		font-size: 2em;
	}
	.color_key {
		font-size: 1.5em;
	}
	.digit {
		font-size: 2em;
	}

	#key_black {
		border-color: black;
		border-width: 0.2em;
	}
	#key_white {
		border-color: black;
		border-width: 0.2em;
	}
</style>
