<script lang="js">
	import { getContext } from 'svelte';
	import settingsIcon from '$lib/icons/settings.png';
	import cancelIcon from '$lib/icons/cancel.png';

	const toggle = getContext('toggleSettings');
</script>

<button
	class="settings-control"
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

	.settings-control {
		position: absolute;
		opacity: 0.2;
		top: 1rem;
		right: 1rem;
		background: none;
		border: none;
	}

	.settings-control:hover {
		opacity: 0.5;
	}
</style>
