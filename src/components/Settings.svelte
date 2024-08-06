<script lang="js">
	import { getContext } from 'svelte';
	import settingsIcon from '$lib/icons/settings.png';

	const toggle = getContext('toggleSettings');
</script>

<button
	class="settings-button"
	on:click={() => {
		toggle();
	}}
>
	<img class="settings-icon" src={settingsIcon} alt="sound" />
</button>

<style>
	.settings-icon {
		width: 25px;
		height: 25px;
	}

	.settings-button {
		background: none;
		border: none;
	}
</style>
