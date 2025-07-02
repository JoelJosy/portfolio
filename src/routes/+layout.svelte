<script lang="ts">
	import '../app.css';
	import Lenis from 'lenis';
	import { onMount } from 'svelte';

	let { children } = $props();

	// Initialize Lenis for smooth scrolling
	onMount(() => {
		const lenis = new Lenis({
			lerp: 0.1, // Adjust for more/less smoothing
		});

		function raf(time: number) {
			lenis.raf(time);
			requestAnimationFrame(raf);
		}
		requestAnimationFrame(raf);

		return () => {
			lenis.destroy();
		};
	});
</script>

{@render children()}
