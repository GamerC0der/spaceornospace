{#each Array(256) as _, i}
	{@const row = Math.floor(i / 16)}
	{@const enabled = row === 0 || row === 15}
	<div class="tile" class:enabled style="left: calc({i % 16} * var(--tile-width)); top: calc({row} * var(--tile-height))"></div>
{/each}

<img
	src="/meeple-blue.png"
	alt="Meeple"
	class:position-center={position === 'center'}
	class:position-top={position === 'top'}
	class:position-bottom={position === 'bottom'}
/>

<script>
	let position = $state('center');

	function handleKeydown(event) {
		if (event.code === 'Space') {
			event.preventDefault();
			position = position === 'top' ? 'bottom' : 'top';
		}
	}

	$effect(() => {
		document.addEventListener('keydown', handleKeydown);
		return () => document.removeEventListener('keydown', handleKeydown);
	});
</script>

<style>
	:global(body) {
		background: url('/bg.png') repeat;
		margin: 0;
		padding: 0;
		height: 100vh;
		position: relative;
	}

	img {
		width: 320px;
		height: 320px;
		image-rendering: pixelated;
		image-rendering: -moz-crisp-edges;
		image-rendering: crisp-edges;
		position: absolute;
		left: 50%;
		transform: translateX(-50%);
	}

	.position-center {
		top: 50%;
		transform: translate(-50%, -50%);
	}

	.position-top {
		top: 10%;
	}

	.position-bottom {
		top: 90%;
		transform: translateX(-50%) translateY(-100%);
	}

	.tile {
		--tile-width: calc(100vw / 16);
		--tile-height: calc(100vh / 16);
		width: var(--tile-width);
		height: var(--tile-height);
		position: absolute;
		border: 1px solid rgba(255, 255, 255, 0.3);
		box-sizing: border-box;
	}

	.tile.enabled {
		background-color: rgba(255, 255, 255, 0.1);
	}
</style>
