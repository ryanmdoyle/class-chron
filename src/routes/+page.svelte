<script lang="ts">
	import { onMount, setContext } from 'svelte';
	import { fade } from 'svelte/transition';

	import Timer from '../components/Timer.svelte';
	import Settings from '../components/Settings.svelte';

	import soundIcon from '$lib/icons/sound.png';
	import noSoundIcon from '$lib/icons/noSound.png';

	let isSound = false;
	let isSettings = false;
	let primaryColor: string = '#23ce7f';
	let secondaryColor: string = '#a451c8';
	let loaded = false;

	setContext('toggleSettings', toggleSettings);

	onMount(() => {
		//check if there is a color saved to localstorage
		const localPrimary = localStorage.getItem('primaryColor');
		const localSecondary = localStorage.getItem('secondaryColor');
		// store colors to memory if nothing saved
		if (!localPrimary || !localSecondary) {
			localStorage.setItem('primaryColor', primaryColor);
			localStorage.setItem('secondaryColor', secondaryColor);
		}

		//if local colors saved, upadate app colors to match
		if (localPrimary) {
			primaryColor = localPrimary;
		}
		if (localSecondary) {
			secondaryColor = localSecondary;
		}
		// set loaded to true -> colors are current for user
		loaded = true;
	});

	function toggleSettings(): void {
		isSettings = !isSettings;
	}

	function toggleSound(): void {
		isSound = !isSound;
	}

	function setPrimary(
		e: Event & {
			currentTarget: EventTarget & HTMLInputElement;
		}
	): void {
		const newValue = e.currentTarget.value;
		if (newValue) {
			primaryColor = newValue;
		}
		localStorage.setItem('primaryColor', primaryColor);
	}

	function setSecondary(
		e: Event & {
			currentTarget: EventTarget & HTMLInputElement;
		}
	): void {
		const newValue = e.currentTarget.value;
		if (newValue) {
			secondaryColor = newValue;
		}
		localStorage.setItem('secondaryColor', secondaryColor);
	}
</script>

{#if loaded}
	<div
		class="window-container"
		style:background={`linear-gradient(135deg, ${primaryColor}, ${secondaryColor})`}
		in:fade={{ duration: 500 }}
	>
		<Timer {isSound} />
		{#if isSettings}
			<div id="primary-color">
				<input
					type="color"
					id="primaryColor"
					name="primaryColor"
					value={primaryColor}
					on:change={(e) => {
						setPrimary(e);
					}}
				/>
				<label for="primary-color"></label>
			</div>

			<div id="secondary-color">
				<input
					type="color"
					id="secondaryColor"
					name="secondaryColor"
					value={secondaryColor}
					on:change={(e) => {
						setSecondary(e);
					}}
				/>
				<label for="secoldary-color"></label>
			</div>
		{/if}
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

		<Settings />
	</div>
{/if}

<style>
	#primary-color,
	#secondary-color {
		position: absolute;
		opacity: 0.2;
	}
	#primary-color {
		top: 1rem;
		left: 1rem;
	}
	#secondary-color {
		bottom: 1rem;
		right: 1rem;
	}
	.window-container {
		display: flex;
		justify-content: center;
		align-items: center;
		width: 100%;
		height: 100vh;
		padding: 0;
	}

	.sound-icon {
		object-fit: contain;
		width: 25px;
		height: 25px;
	}

	.sound-controls {
		position: absolute;
		opacity: 0.2;
		bottom: 1rem;
		left: 1rem;
	}

	.sound-controls button {
		background: none;
		border: none;
	}

	.sound-controls:hover {
		opacity: 0.5;
	}
</style>
