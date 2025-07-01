<script lang="ts">
    import { gsap } from 'gsap';

    interface Props {
        visible?: boolean;
        onComplete?: () => void;
    }

    let { visible = true, onComplete }: Props = $props();
    let loadingContainer: HTMLElement;
    let loadingBar: HTMLElement;
    let loadingText: HTMLElement;

    $effect(() => {
        if (visible) {
            startLoadingAnimation();
        }
    });

    function startLoadingAnimation() {
        // Reset elements
        gsap.set(loadingBar, { width: "0%" });
        gsap.set([loadingText], { opacity: 0, y: 20 });
        
        // Create timeline
        const tl = gsap.timeline({ delay: 0.3 });

        // Animate loading text
        tl.to(loadingText, {
            opacity: 1,
            y: 0,
            duration: 0.8,
            ease: "power3.out"
        });

        // Animate loading bar fill
        tl.to(loadingBar, {
            width: "100%",
            duration: 2.5,
            ease: "power2.inOut"
        }, "-=0.4");

        // Expand bar to full screen width
        tl.to(loadingBar, {
            width: "100vw",
            duration: 0.6,
            ease: "power3.inOut"
        });

        // Expand bar to full screen height
        tl.to(loadingBar, {
            height: "100vh",
            duration: 0.8,
            ease: "power3.inOut"
        });

        // Fade out loading screen and reveal content
        tl.to(loadingContainer, {
            opacity: 0,
            duration: 0.5,
            ease: "power3.inOut",
            onComplete: () => {
                loadingContainer.style.pointerEvents = 'none';
                if (onComplete) onComplete();
            }
        });
    }
</script>

<div class="loading-screen" class:visible bind:this={loadingContainer}>
    <div class="loading-content">
        <div class="loading-text" bind:this={loadingText}>
            Loading
        </div>
    </div>
    
    <div class="loading-bar" bind:this={loadingBar}></div>
</div>

<style>
    .loading-screen {
        position: fixed;
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
        background: #000000;
        display: flex;
        justify-content: center;
        align-items: center;
        z-index: 9999;
        opacity: 1;
        transition: opacity 0.3s ease;
    }

    .loading-screen:not(.visible) {
        opacity: 0;
        pointer-events: none;
    }

    .loading-content {
        position: relative;
        z-index: 2;
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 2rem;
    }

    .loading-text {
        font-size: clamp(1.5rem, 3vw, 2rem);
        font-weight: 300;
        color: #ffffff;
        letter-spacing: 0.1em;
        font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
        text-transform: uppercase;
    }

    .loading-bar {
        position: absolute;
        bottom: 0;
        left: 0;
        width: 0%;
        height: 4px;
        background: #ffffff;
        z-index: 1;
    }

    @media (max-width: 768px) {
        .loading-content {
            gap: 1.5rem;
        }
    }

    @media (max-width: 480px) {
        .loading-content {
            gap: 1.25rem;
        }
    }
</style>
