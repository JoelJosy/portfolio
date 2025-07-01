<script lang="ts">
    import LoadingScreen from '$lib/components/LoadingScreen.svelte';
    import Hero from '$lib/components/Hero.svelte';
    import Navbar from '$lib/components/Navbar.svelte';
    import ProjectsIntro from '$lib/components/ProjectsIntro.svelte';
    import ProjectsSection from '$lib/components/ProjectsSection.svelte';
    import AboutSection from '$lib/components/AboutSection.svelte';
    import ContactSection from '$lib/components/ContactSection.svelte';
    import Footer from '$lib/components/Footer.svelte';

    let showContent = false;

    function handleLoadingComplete() {
        showContent = true;
    }
</script>

<Navbar />
<Hero visible={showContent} />
<AboutSection />
<ProjectsIntro />
<ProjectsSection />
<ContactSection />
<Footer />

<!-- <LoadingScreen onComplete={handleLoadingComplete} /> -->