<script>
	// import {createEventDispatcher } from 'svelte'

	// const dispatch = createEventDispatcher();

	// function startGame(){
	//     dispatch('start',{
	//         text:"Started Game"
	//     })
	// }
	import banner from '$lib/images/Banners_Shadow_Pin_Entry.png';
</script>

<!-- <h1>Black and White PIN Entry</h1> -->
<img src={banner} alt="Shadow PIN Entry" width="100%" height="30%"/>

<buton id="start" on:click>Start Game!</buton>

<style>
	#start {
		background-color: aquamarine;
		min-width: 80vw;
		min-height: 5vh;
		text-align: center;
		font-size: 1.5em;
	}
</style>
