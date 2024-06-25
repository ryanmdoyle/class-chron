<script lang="ts">
	import { onMount } from 'svelte';

	let time: string;

	// Function to format the time in HH:MM AM/PM format
	function formatTime(date: Date): string {
		let hours = date.getHours();
		let minutes = date.getMinutes();
		const ampm = hours >= 12 ? 'PM' : 'AM';
		hours = hours % 12;
		hours = hours ? hours : 12; // the hour '0' should be '12'
		const minutesStr = minutes < 10 ? '0' + minutes : minutes.toString();
		return `${hours}:${minutesStr} ${ampm}`;
	}

	function updateTime() {
		const now = new Date();
		time = formatTime(now);
	}

	onMount(() => {
		updateTime();
		const interval = setInterval(updateTime, 1000);
		return () => clearInterval(interval);
	});
</script>

<div class="clock">
	{time}
</div>

<style>
	.clock {
		position: absolute;
		top: 1rem;
		margin: auto;
		color: #ffffff;
		font-family: 'Roboto Mono', monospace;
		font-size: 2rem;
		font-optical-sizing: auto;
		font-weight: 700;
		font-style: normal;
	}
</style>
