<script>
	import { fade, fly, slide, scale, blur } from "svelte/transition";
	import * as eases from "svelte/easing";

	let showing = true;
	let currentEasing = eases.elasticOut;
	let duration = 1000;

	let transitions = {
	  fade: fade,
	  fly: (node, params) => fly(node, { x: 50, ...params }),
	  slide: slide,
	  scale: scale,
	  blur: blur
	};
</script>
<h1>Svelte transitions</h1>

<div class='controls'>
	<div class='input-group'>
		<label for='showing'>
			show
		</label> 
		<input id='showing' type="checkbox" bind:checked={showing} />
	</div>
	
	<div class='input-group'>
		<label for='duration'>
			duration: {duration}
		</label> 
		<input id='duration' type="range" bind:value={duration} min="50" max="2000" step="50" />
	</div>
	
	<div class='input-group'>
		<label for='easing'>
			easing
		</label>
		<select id='easing' bind:value={currentEasing}>
		{#each Object.entries(eases) as [name, func]}
			<option value={func}>
				{name}
			</option>
		{/each}
		</select>
	</div>
</div>

{#if showing}
	<section>
	{#each Object.entries(transitions) as [name, func] (name)} 
		<div>
			<div class='box' in:func={{easing: currentEasing, duration}} out:func={{easing: currentEasing, duration}} />
			<p style="font-weight: bold; font-family: monospace;">{name}</p>
		</div>
	{/each}
	</section>
{/if}

<style>
	h1{
		text-align: center;
	}
	section {
	  display: flex;
	  flex-wrap: wrap;
	  justify-content: center;
	  gap: 2rem;
	  margin-top: 5rem;
	}

	.controls {
	  display: flex;
	  justify-content: center;
	  gap: 2rem;
	  font-family: sans-serif;
	}

	.input-group {
	  display: flex;
	  flex-direction: column;
	  align-items: baseline;
	  gap: 0.5rem;
	}

	.box {
	  width: 100px;
	  height: auto;
	  aspect-ratio: 1 / 1;
	  background: goldenrod;
	  border-radius: 10px;
	  box-shadow: 1px 3px 6px 1px lightgrey;
	}
</style>