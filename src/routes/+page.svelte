<script lang="ts">
    import LoadingScreen from '$lib/components/LoadingScreen.svelte';
    import Hero from '$lib/components/Hero.svelte';

    let showContent = false;

    function handleLoadingComplete() {
        showContent = true;
    }
</script>

<Hero visible={showContent} />
<LoadingScreen onComplete={handleLoadingComplete} />