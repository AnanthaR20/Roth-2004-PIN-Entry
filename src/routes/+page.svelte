<script lang="ts">
	import KeyPad from './KeyPad.svelte';

	let L;
	let R;
	let O;
	let P;

	let candidates = [];
	let non_candidates = [];
	function rand_digit(): number {
		return(Math.floor(Math.random()*10))
	}
	function rand_digit_min_i(i: number): number {
		for (;;) {
			const j = rand_digit();
			if (j >= i) {
				return j;
			}
		}
	}
	function rand_perm(): Array<number> {
		let array = new Array<number>(10);
		for (var i = 0; i < 10; ++i) {
			array[i] = i;
		}
		for (i = 0; i < 9; ++i) {
			const j = rand_digit_min_i(i);
			const temp = array[i];
			array[i] = array[j];
			array[j] = temp;
		}
		return array;
	}
	function gamma(array:Array<number>): Array<Array<number>> {
		let midpoint = Math.ceil(array.length/2);
		let L = array.slice(0,midpoint)
		let R = array.slice(midpoint,array.length);
		return([L,R])
	}

</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	<KeyPad key_colors={new Uint8Array([1,1,1,0,1,0,0,0,0,1])} training_board={false}/>
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	h1 {
		width: 100%;
	}

	.welcome {
		display: block;
		position: relative;
		width: 100%;
		height: 0;
		padding: 0 0 calc(100% * 495 / 2048) 0;
	}

	.welcome img {
		position: absolute;
		width: 100%;
		height: 100%;
		top: 0;
		display: block;
	}
</style>
