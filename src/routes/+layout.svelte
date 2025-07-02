<script lang="ts">
	import '../app.css';
	import Lenis from 'lenis';
	import SquishyCursor from '$lib/components/SquishyCursor.svelte';

	let { children } = $props();

	// Initialize Lenis for smooth scrolling
	$effect(() => {
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

<SquishyCursor />

{@render children()}
