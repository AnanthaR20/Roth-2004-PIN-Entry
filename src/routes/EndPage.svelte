<script lang="ts">
	import banner from '$lib/images/Banners_Shadow_Pin_Entry.png';
	
	export let PIN_visible = false;
	export let PIN = '';
</script>

<!-- <h1>Black and White PIN Entry</h1> -->
<img src={banner} alt="Shadow PIN Entry" width="100%" height="30%"/>
<br>

{#if PIN_visible}
	<p>{PIN}</p>
{/if}
<button
	id="TogglePIN"
	on:click={() => {
		PIN_visible = !PIN_visible;
	}}>Toggle PIN Visibility</button
>
