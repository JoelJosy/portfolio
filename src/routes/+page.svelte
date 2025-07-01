<script lang="ts">
    import LoadingScreen from '$lib/components/LoadingScreen.svelte';
    import Hero from '$lib/components/Hero.svelte';
    import Navbar from '$lib/components/Navbar.svelte';
    import ProjectsIntro from '$lib/components/ProjectsIntro.svelte';

    let showContent = false;

    function handleLoadingComplete() {
        showContent = true;
    }
</script>

<Navbar />
<Hero visible={showContent} />
<ProjectsIntro />
<!-- <LoadingScreen onComplete={handleLoadingComplete} /> -->