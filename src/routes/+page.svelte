<script lang="ts">
	import StartPage from "./StartPage.svelte";
	import KeyPad from './KeyPad.svelte';
	import EndPage from "./EndPage.svelte";
	let show_start_screen = true;
	let show_keypad_screen = false;
	let show_end_screen = false;

	// variables for executing B&W Roth Algo
	let rounds= 0;
	let userEnteredPIN = '';
	let candidates:Array<number> = [0,1,2,3,4,5,6,7,8,9];
	let non_candidates:Array<number> = [];
	let displayDigits = [0,0,0,0,0,0,0,0,0,0];

	function indicesOf(arr:Array<number>,val:number):Array<number> {
    	let indexArr:Array<number> = [];
    	arr.forEach((element, index) => {
        	if (element === val) {
	            indexArr.push(index);
        	}
    	});
    	return indexArr;
	}
	function shuffle(array:Array<number>) {
  		let currentIndex = array.length,  randomIndex;
  		// While there remain elements to shuffle.
  		while (currentIndex > 0) {
    		// Pick a remaining element.
    		randomIndex = Math.floor(Math.random() * currentIndex);
    		currentIndex--;
    		// And swap it with the current element.
    		[array[currentIndex], array[randomIndex]] = [
      		array[randomIndex], array[currentIndex]];
  		}
  		return array;
	}	
	function cutInHalf(array:Array<number>): Array<Array<number>> {
		let midpoint = Math.ceil(array.length/2);
		let L = array.slice(0,midpoint)
		let R = array.slice(midpoint,array.length);
		return([L,R])
	}
	function intersect(a:Array<number>,b:Array<number>){
		return a.filter(value=>b.includes(value))
	}
	function setDiff(a:Array<number>,b:Array<number>){
		return a.filter(function(x) { return b.indexOf(x) < 0; });
	}
	function gameHeader(rounds:number){
		let a = ''
		switch(Math.floor(rounds/4)+1){
			case 1: {a ="1st"; break;}
			case 2: {a ="2nd";break;}
			case 3: {a ="3rd";break;}
			case 4: {a ="4th";break;}
		}
		return a
	}

	function convertToDisplayDigits(L:Array<number>){
		// Takes in 2 arrays L,R. Colors L black and 'not' L white
		let coloring_key = []
		for(let i=0;i<10;i++){
			L.indexOf(i) !== -1 ? coloring_key.push(0) : coloring_key.push(1);
		}
		return coloring_key
	}
	function setNextRoundKeyPadColoring(){
		let LR = cutInHalf(shuffle(candidates));
			let L = LR[0];
			let R = LR[1];
		let OP = cutInHalf(shuffle(non_candidates));
			let O = OP[0];
			let P = OP[1];
		let L_display = L.concat(P);
		let R_display = R.concat(O);

		displayDigits = convertToDisplayDigits(L_display)
	}
	function play_a_round(event:CustomEvent){
		let BW = event.detail.value;
		if(rounds % 4 === 0){
			candidates = [0,1,2,3,4,5,6,7,8,9]
		}
		candidates = intersect(candidates,indicesOf(displayDigits,BW));
		non_candidates = setDiff([0,1,2,3,4,5,6,7,8,9],candidates);
		// Add the PIN digit when sufficient rounds have passed
		if(rounds % 4 === 3){ 
			let digitUndefined = candidates[0] === undefined
			let digit = digitUndefined ? '#' : String(candidates[0])
			userEnteredPIN = userEnteredPIN.concat(digit);
		}
		// Log the completed round
		console.log("\nPIN Digit #"+Math.floor((rounds/4)+1))
		console.log("Round "+(rounds+1)+". Candidates so far are: " + candidates)
		console.log("PIN entered so far is: " + userEnteredPIN)		
		console.log()
		rounds += 1;
		// handle end of game
		if(userEnteredPIN.length < 4){
			setNextRoundKeyPadColoring()
		} else {
			endGame()
		}
	}
	function startGame(){
		setNextRoundKeyPadColoring();
		show_start_screen = false;
		show_keypad_screen = true;
	}
	function endGame(){
		show_keypad_screen = false;
		show_end_screen = true;
	}
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	{#if show_start_screen}
		<StartPage on:click={startGame}/>
	{/if}
	{#if show_keypad_screen}
		<h1>Tap the color of your <strong><u>{gameHeader(rounds)} digit</u></strong> for <strong>{4-(rounds % 4)}</strong> more rounds.</h1>
		<KeyPad key_colors={displayDigits} training_board={true} on:BW_input={play_a_round}/>
	{/if}
	{#if show_end_screen}
		<EndPage PIN = {userEnteredPIN} />
	{/if}


	
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
