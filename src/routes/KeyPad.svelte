<script lang="ts">
	const color = ["#666666","white"]
	let key_colors:Uint8Array = new Uint8Array([0,0,0,0,0,1,1,1,1,1]);
</script>

<div class="keypad" style =
"--key1_color">
	<button class="key digit" id="key1" style="background-color:{color[key_colors[0]]}">1</button>
	<button class="key digit" id="key2" style="background-color:{color[key_colors[1]]}">2</button>
	<button class="key digit" id="key3" style="background-color:{color[key_colors[2]]}">3</button>
	<button class="key digit" id="key4" style="background-color:{color[key_colors[3]]}">4</button>
	<button class="key digit" id="key5" style="background-color:{color[key_colors[4]]}">5</button>
	<button class="key digit" id="key6" style="background-color:{color[key_colors[5]]}">6</button>
	<button class="key digit" id="key7" style="background-color:{color[key_colors[6]]}">7</button>
	<button class="key digit" id="key8" style="background-color:{color[key_colors[7]]}">8</button>
	<button class="key digit" id="key9" style="background-color:{color[key_colors[8]]}">9</button>
	<button class="key" id="key_backspace">Backspace</button>
	<button class="key digit" id="key0" style="background-color:{color[key_colors[9]]}">0</button>
	<button class="key" id="key_enter">Enter</button>
</div>

<style>
	.keypad {
		display: grid;
		grid-template-columns: repeat(3, 1fr);
		grid-auto-rows: minmax(100px, auto);
		border-style: solid;
	}
	.key {
		display: flex;
		border-style: solid;
		background: white;
		align-items: center;
		justify-content: center;
	}
	.digit {
		font-size: 2em;
	}

	#key1 {
		background: var(--key1_color);
	}
	#key2 {
		background: var(--key2_color);
	}
	#key3 {
		background: var(--key3_color);
	}
	#key4 {
		background: var(--key4_color);
	}
	#key5 {
		background: var(--key5_color);
	}
	#key6 {
		background: var(--key6_color);
	}
	#key7 {
		background: var(--key7_color);
	}
	#key8 {
		background: var(--key8_color);
	}
	#key9 {
		background: var(--key9_color);
	}
	#key0 {
		background: var(--key0_color);
	}
</style>
