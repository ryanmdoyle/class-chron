<script lang="ts">
	import soundIcon from '$lib/icons/sound.png';
	import noSoundIcon from '$lib/icons/noSound.png';
	// import highBell from '$lib/sounds/highBell.mp3';
	let audio: HTMLAudioElement;
	let total = 3;
	let timer: number;
	let running = false;
	let isSound = false;

	function setTotal(minutes: number): void {
		if (minutes >= 0) {
			total = minutes * 60;
		}
	}

	function secondsDisplay(total: number): string {
		if (total % 60 > 9) {
			const remainging = total % 60;
			return remainging.toString();
		} else {
			return `0${total % 60}`;
		}
	}

	function minutesDisplay(total: number): string {
		const min = Math.floor(total / 60);
		if (min < 10) {
			return '0' + min.toString();
		}
		return min.toString();
	}

	function startTimer(): void {
		if (!running && total > 0) {
			running = true;
			timer = setInterval(() => {
				console.log(total);
				if (total == 1) {
					if (isSound) {
						audio.play();
					}
					total = total - 1;
					stopTimer();
				} else {
					total = total - 1;
				}
			}, 1000);
		}
	}

	function stopTimer(): void {
		if (running) {
			running = false;
			clearInterval(timer);
		}
	}

	function clearTimer(timer: number): void {
		total = 0;
		running = false;
		clearInterval(timer);
		if (isSound) {
			audio.play();
		}
	}

	function addMin(): void {
		total += 60;
	}

	function subMin(timer: number): void {
		if (total - 60 >= 0) {
			total -= 60;
		} else {
			clearTimer(timer);
		}
	}

	function toggleSound(): void {
		isSound = !isSound;
	}

	function changeAudio(): void {}
</script>

<div class="window-container">
	<div class="timer-container">
		<div class="timer-display">
			<h1 class="roboto-mono-700">{minutesDisplay(total)}:{secondsDisplay(total)}</h1>
		</div>
		<div class="timer-controls roboto-mono-400">
			{#if running}
				<button class="roboto-mono-400" id="stop-btn" on:click={() => stopTimer()}>Stop</button>
			{:else}
				<button class="roboto-mono-400" on:click={() => startTimer()}>Start</button>
			{/if}
			<button class="roboto-mono-400" on:click={() => clearTimer(timer)}>Clear</button>
			<button class="roboto-mono-400" on:click={() => subMin(timer)}>-</button>
			<button class="roboto-mono-400" on:click={() => addMin()}>+</button>
			<button class="roboto-mono-400" on:click={() => setTotal(1)}>1</button>
			<button class="roboto-mono-400" on:click={() => setTotal(2)}>2</button>
			<button class="roboto-mono-400" on:click={() => setTotal(3)}>3</button>
			<button class="roboto-mono-400" on:click={() => setTotal(5)}>5</button>
			<button class="roboto-mono-400" on:click={() => setTotal(10)}>10</button>
			<button class="roboto-mono-400" on:click={() => setTotal(15)}>15</button>
			<button class="roboto-mono-400" on:click={() => setTotal(20)}>20</button>
		</div>
	</div>
	<audio src="https://class-chron.s3.us-west-1.amazonaws.com/bell.mp3" bind:this={audio}></audio>
	<div class="sound-controls">
		{#if isSound}
			<button on:click={toggleSound}>
				<img class="sound-icon" src={soundIcon} alt="sound" />
			</button>
		{:else}
			<button on:click={toggleSound}>
				<img class="sound-icon" src={noSoundIcon} alt="no sound" />
			</button>
		{/if}
	</div>
</div>

<style>
	h1 {
		margin: 0 auto;
	}
	.window-container {
		display: flex;
		justify-content: center;
		align-items: center;
		width: 100%;
		height: 100vh;
		padding: 0;
		background: linear-gradient(135deg, rgb(35, 206, 127), rgb(164, 81, 200));
	}

	.timer-container {
		display: flex;
		flex-direction: column;
	}

	.timer-display {
		display: flex;
		justify-content: center;
		font-size: 6rem;
		color: white;
	}

	.timer-controls {
		display: flex;
		justify-content: space-evenly;
		margin-bottom: 1rem;
	}

	.timer-controls button {
		background-color: transparent;
		border: 1px solid rgba(255, 255, 255, 0.25);
		border-radius: 10px;
		padding: 1rem;
		color: rgba(255, 255, 255, 0.25);
		font-size: 2rem;
		-webkit-transition: all 0.3s ease-out;
		-moz-transition: all 0.3s ease-out;
		-o-transition: all 0.3s ease-out;
		transition: all 0.3s ease-out;
	}

	.timer-controls button:hover {
		border: 1px solid rgba(255, 255, 255, 0.5);
		color: rgba(255, 255, 255, 0.5);
		background-color: rgba(255, 255, 255, 0.1);
	}

	.timer-controls button:not(:last-child) {
		margin-right: 1rem;
	}

	#stop-btn {
		/* 1 rem (existing padding from timer-controls) + 1/2ch for difference with "start".length*/
		padding-left: calc(1rem + 0.5ch);
		padding-right: calc(1rem + 0.5ch);
	}

	.sound-icon {
		object-fit: contain;
		width: 50px;
		height: 50px;
	}

	.sound-controls {
		position: absolute;
		opacity: 0.2;
		bottom: 15px;
		right: 15px;
	}

	.sound-controls button {
		background: none;
		border: none;
	}

	.sound-controls:hover {
		opacity: 0.5;
	}

	.roboto-mono-400 {
		font-family: 'Roboto Mono', monospace;
		font-optical-sizing: auto;
		font-weight: 400;
		font-style: normal;
	}

	.roboto-mono-700 {
		font-family: 'Roboto Mono', monospace;
		font-optical-sizing: auto;
		font-weight: 700;
		font-style: normal;
	}
</style>
