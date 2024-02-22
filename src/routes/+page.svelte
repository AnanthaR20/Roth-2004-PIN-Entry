<script lang="ts">
	// import StartPage from './StartPage.svelte';
	import KeyPad from './KeyPad.svelte';
	// import EndPage from './EndPage.svelte';
	import type { CreateInstanceResponse, UpdateInstanceResponse, GetPointsResponse } from './logic';

	// Game Initial Values
	let show_start_screen = true;
	let show_keypad_screen = false;
	let show_end_screen = false;
	let show_pin = false;
	let rounds = 0;
	let userEnteredPIN = '';
	let candidates: Array<number> = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
	let non_candidates: Array<number> = [];
	let displayDigits = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0];

	function indicesOf(arr: Array<number>, val: number): Array<number> {
		let indexArr: Array<number> = [];
		arr.forEach((element, index) => {
			if (element === val) {
				indexArr.push(index);
			}
		});
		return indexArr;
	}
	function shuffle(array: Array<number>) {
		let currentIndex = array.length,
			randomIndex;
		// While there remain elements to shuffle.
		while (currentIndex > 0) {
			// Pick a remaining element.
			randomIndex = Math.floor(Math.random() * currentIndex);
			currentIndex--;
			// And swap it with the current element.
			[array[currentIndex], array[randomIndex]] = [array[randomIndex], array[currentIndex]];
		}
		return array;
	}
	function cutInHalf(array: Array<number>): Array<Array<number>> {
		let midpoint = Math.ceil(array.length / 2);
		let L = array.slice(0, midpoint);
		let R = array.slice(midpoint, array.length);
		return [L, R];
	}
	function intersect(a: Array<number>, b: Array<number>) {
		return a.filter((value) => b.includes(value));
	}
	function setDiff(a: Array<number>, b: Array<number>) {
		return a.filter(function (x) {
			return b.indexOf(x) < 0;
		});
	}
	function gameHeader(rounds: number) {
		let a = '';
		switch (Math.floor(rounds / 4) + 1) {
			case 1: {
				a = '1st';
				break;
			}
			case 2: {
				a = '2nd';
				break;
			}
			case 3: {
				a = '3rd';
				break;
			}
			case 4: {
				a = '4th';
				break;
			}
		}
		return a;
	}

	function convertToDisplayDigits(L: Array<number>) {
		// Takes in 2 arrays L,R. Colors L black and 'not' L white
		let coloring_key = [];
		for (let i = 0; i < 10; i++) {
			L.indexOf(i) !== -1 ? coloring_key.push(0) : coloring_key.push(1);
		}
		return coloring_key;
	}
	function setNextRoundKeyPadColoring() {
		let LR = cutInHalf(shuffle(candidates));
		let L = LR[0];
		// let R = LR[1];
		let OP = cutInHalf(shuffle(non_candidates));
		// let O = OP[0];
		let P = OP[1];
		let L_display = L.concat(P);
		// let R_display = R.concat(O);

		displayDigits = convertToDisplayDigits(L_display);
	}
	function play_a_round(event: CustomEvent) {
		let BW = event.detail.value;
		if (rounds % 4 === 0) {
			candidates = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
		}
		candidates = intersect(candidates, indicesOf(displayDigits, BW));
		non_candidates = setDiff([0, 1, 2, 3, 4, 5, 6, 7, 8, 9], candidates);
		// Add the PIN digit when sufficient rounds have passed
		if (rounds % 4 === 3) {
			let digitUndefined = candidates[0] === undefined;
			let digit = digitUndefined ? '#' : String(candidates[0]);
			userEnteredPIN = userEnteredPIN.concat(digit);
		}
		// Log the completed round
		console.log('\nPIN Digit #' + Math.floor(rounds / 4 + 1));
		console.log('Round ' + (rounds + 1) + '. Candidates so far are: ' + candidates);
		console.log('PIN entered so far is: ' + userEnteredPIN);
		console.log();
		rounds += 1;
		// handle end of game
		if (userEnteredPIN.length < 4) {
			setNextRoundKeyPadColoring();
		} else {
			endGame();
		}
	}
	function startGame() {
		setNextRoundKeyPadColoring();
		show_start_screen = false;
		show_keypad_screen = true;
	}
	function endGame() {
		finish_transition();
	}

	let uid: string = '';
	let uid_valid: boolean = false;
	$: uid_valid = check_uid_valid(uid);
	let iid: number = -1;
	let actual_pin: string = '';

	function reset() {
		show_start_screen = true;
		show_keypad_screen = false;
		show_end_screen = false;
		show_pin = false;
		rounds = 0;
		userEnteredPIN = '';
		candidates = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
		non_candidates = [];
		displayDigits = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0];

		show_start_screen = true;
		show_pin = false;
		uid_valid = false;
		iid = -1;
		actual_pin = '';
		const temp = uid;
		uid = '';
		uid = temp;
	}

	async function get_points(): Promise<GetPointsResponse> {
		const url = `https://142.93.219.243.nip.io/points/${uid.toLowerCase()}`;
		const request = new Request(url, { method: 'GET' });
		const data = await fetch(request);
		return <GetPointsResponse>(<unknown>data.json());
	}

	function normalize() {
		uid = uid;
	}

	function set_actual_pin(val: string) {
		actual_pin = val;
	}

	function isAlphaNumeric(str: string): boolean {
		let code, i, len;
		for (i = 0, len = str.length; i < len; i++) {
			code = str.charCodeAt(i);
			if (!(code > 47 && code < 58) && !(code > 64 && code < 91) && !(code > 96 && code < 123)) {
				return false;
			}
		}
		return true;
	}

	function check_uid_valid(uid_cand: string): boolean {
		return uid_cand.length === 6 && isAlphaNumeric(uid_cand);
	}

	function progress_transition() {
		if (!uid_valid) return;
		const url = 'https://142.93.219.243.nip.io/create_instance';
		const data = {
			game_id: 'R',
			user_id: uid.toLowerCase()
		};
		const request = new Request(url, {
			method: 'POST',
			body: JSON.stringify(data),
			headers: new Headers({
				'Content-Type': 'application/json; charset=UTF-8'
			})
		});
		fetch(request).then((create_instance_value_temp) => {
			create_instance_value_temp.json().then((temp) => {
				const create_instance_value = <CreateInstanceResponse>(<unknown>temp);
				iid = create_instance_value.iid;
				startGame();
			});
		});
	}

	function finish_transition() {
		const url = 'https://142.93.219.243.nip.io/update_instance';
		const data = {
			iid_value: iid,
			result_pin: userEnteredPIN
		};
		const request = new Request(url, {
			method: 'POST',
			body: JSON.stringify(data),
			headers: new Headers({
				'Content-Type': 'application/json; charset=UTF-8'
			})
		});
		fetch(request).then((instance_response_value_temp) => {
			instance_response_value_temp.json().then((temp) => {
				const instance_response_value = <UpdateInstanceResponse>(<unknown>temp);
				if (iid !== instance_response_value.iid) {
					alert('Reached invalid state, please report bug!');
				}
				show_end_screen = true;
				show_keypad_screen = false;
			});
		});
	}
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	{#if show_start_screen}
		<input
			type="text"
			placeholder="User ID"
			bind:value={uid}
			on:change={normalize}
			maxlength="6"
			name="userid"
			id="userid"
		/>
		{#if uid_valid}
			{#await get_points()}
				<p>Validating User ID...</p>
			{:then get_points_value}
				{#if get_points_value.uid !== null}
					{(set_actual_pin(get_points_value.actual_pin), '')}
					<p style="color: green">Points: {get_points_value.points}</p>
				{:else}
					<p style="color: red">Invalid User ID!</p>
				{/if}
			{:catch error}
				{(console.log(error), '')}
				<p style="color: purple">Network Error: Unable to check validity of User ID!</p>
			{/await}
		{/if}
		<br />
		<button on:click={progress_transition}>Start Game</button>
		<!-- <StartPage on:click={() => startGame()}/> -->
	{/if}
	{#if show_keypad_screen}
		<h1><strong><u>{gameHeader(rounds)} digit:</u></strong> Round {(rounds % 4) + 1}</h1>
		<KeyPad key_colors={displayDigits} training_board={true} on:BW_input={play_a_round} />
		<h1>Tap color of your <strong><u>{gameHeader(rounds)} digit</u></strong></h1>
	{/if}
	{#if show_end_screen}
		{#if show_pin}
			<div>Entered PIN: {userEnteredPIN}</div>
		{/if}
		<br />
		{#if actual_pin === userEnteredPIN}
			<p style="color: green">Congratulations on entering the correct PIN!</p>
		{:else}
			<p style="color: red">Incorrect PIN entered, no points earned!</p>
		{/if}
		<br />
		{#await get_points()}
			<p>Fetching points...</p>
		{:then get_points_value}
			{#if get_points_value.uid !== null}
				<p style="color: green">Points: {get_points_value.points}</p>
			{:else}
				<p style="color: red">Invalid User ID!</p>
			{/if}
		{:catch error}
			{(console.log(error), '')}
			<p style="color: purple">Network Error: Unable to fetch points!</p>
		{/await}
		<br />
		<button on:click={() => (show_pin = !show_pin)}>Toggle Visibility of Entered PIN</button>
		<br />
		<button on:click={reset}>Play Again</button>
		<br />
		<button><a href="https://142.93.219.243.nip.io/">Checkout Other Games</a></button>
		<!-- <EndPage PIN={userEnteredPIN}/> -->
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
</style>
