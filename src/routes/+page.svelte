<script lang="ts">
	import { onMount } from 'svelte'
	import { scale } from 'svelte/transition'

	let count = 0
	let number = 20

	onMount(() => {
		const timeout = setTimeout(() => (count = 1), 500)
		const internal = setInterval(() => count++, 7000)
		return () => {
			clearTimeout(timeout)
			clearInterval(internal)
		}
	})
</script>

<input type="range" bind:value={number} max={50} />
<div style={`--state: ${count * 0.1}`} class="host">
	{#each Array.from({ length: number }) as _, i}
		<div transition:scale class="square" class:odd={i % 2 === 0} style={`--index: ${i + 1}`} />
	{/each}
</div>

<style>
	.host {
		display: grid;

		align-items: center;
		justify-content: center;
		justify-items: center;
		--rotation: 135deg;
		--rotation: 225deg;
		--size-step: 10px;
		--odd-color-step: 5;
		--even-color-step: 5;
		--center: 12;

		width: 100%;
		height: 500px;

		contain: strict;
	}

	input {
		width: 100%;
	}

	.square {
		--size: calc(40px + var(--index) * var(--size-step));
		z-index: calc(var(--index) * -1);
		display: block;
		width: var(--size);
		height: var(--size);
		transform: rotateZ(calc(var(--rotation) * var(--state) * (var(--center) - var(--index))));
		transition-property: transform, border-color;
		transition-duration: 5s;
		transition-timing-function: ease-in-out;
		grid-area: 1 / 1;
		background: white;
		border-width: 2px;
		border-style: solid;
		border-color: black;
		box-sizing: border-box;
		will-change: transform, border-color;

		contain: strict;
	}

	.square.odd {
		--luminance: calc(1 - calc(calc(var(--index) * var(--odd-color-step)) / 256));
		background: rgb(calc(172 * var(--luminance)), calc(127 * var(--luminance)), calc(244 * var(--luminance)));
	}
</style>
