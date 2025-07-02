<script lang="ts">
    import { gsap } from 'gsap';
    
    interface Props {
        visible?: boolean;
    }

    let { visible = false }: Props = $props();
    let heroContainer: HTMLElement;
    let nameElement: HTMLElement;
    let aboutElement: HTMLElement;
    let scrollIndicator: HTMLElement;
    let socialLinks: HTMLElement;

    $effect(() => {
        // Set initial states consistent with other sections
        gsap.set([nameElement, aboutElement, socialLinks], {
            y: 60,
            opacity: 0
        });

        gsap.set(scrollIndicator, {
            y: 40,
            opacity: 0
        });

        // Create timeline for entrance animations with consistent timing
        const tl = gsap.timeline({ delay: 0.5 });

        // Animate name with consistent timing (like other titles)
        tl.to(nameElement, {
            y: 0,
            opacity: 1,
            duration: 1.2,
            ease: "power3.out"
        });

        // Animate about text and social links together
        tl.to([aboutElement, socialLinks], {
            y: 0,
            opacity: 1,
            duration: 1.2,
            ease: "power3.out"
        }, "-=0.8");

        // Animate scroll indicator last
        tl.to(scrollIndicator, {
            y: 0,
            opacity: 1,
            duration: 0.8,
            ease: "power2.out"
        }, "-=0.4");

        // Floating animation for scroll indicator
        gsap.to(scrollIndicator, {
            y: 15,
            duration: 1.5,
            repeat: -1,
            yoyo: true,
            ease: "power2.inOut",
            delay: 3
        });
    });
</script>

<div class="hero" bind:this={heroContainer} id="home">
    <div class="content-wrapper">
        <div class="name-role-group">
            <div class="name" bind:this={nameElement}>
                <div class="name-line">Joel Josy</div>
            </div>
            
            <div class="role-section">
                <p class="role" bind:this={aboutElement}>
                    Full-Stack Web Developer & Designer
                </p>
            </div>
        </div>

        <div class="social-links" bind:this={socialLinks}>
            <a href="mailto:joeljosy79@gmail.com" class="social-link" aria-label="Email">
                <span class="social-text">Email</span>
            </a>
            <a href="https://github.com/JoelJosy" class="social-link" aria-label="GitHub">
                <span class="social-text">GitHub</span>
            </a>
            <a href="https://linkedin.com/in/joel-josy" class="social-link" aria-label="LinkedIn">
                <span class="social-text">LinkedIn</span>
            </a>
            <a href="https://twitter.com/joeljosy" class="social-link" aria-label="Twitter">
                <span class="social-text">Twitter</span>
            </a>
        </div>
    </div>

    <div class="scroll-indicator" bind:this={scrollIndicator}>
        <div class="chevrons">
            <svg class="chevron chevron1" width="24" height="8" viewBox="0 0 24 8" fill="none" xmlns="http://www.w3.org/2000/svg">
                <polyline points="2,2 12,6 22,2" stroke="#b0b0b0" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" fill="none"/>
            </svg>
            <svg class="chevron chevron2" width="24" height="8" viewBox="0 0 24 8" fill="none" xmlns="http://www.w3.org/2000/svg">
                <polyline points="2,2 12,6 22,2" stroke="#b0b0b0" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" fill="none"/>
            </svg>
            <svg class="chevron chevron3" width="24" height="8" viewBox="0 0 24 8" fill="none" xmlns="http://www.w3.org/2000/svg">
                <polyline points="2,2 12,6 22,2" stroke="#b0b0b0" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" fill="none"/>
            </svg>
        </div>
    </div>
</div>

<style>
    .hero {
        position: relative;
        width: 100vw;
        height: 100vh;
        background: #ffffff;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        overflow: hidden;
        padding: 0 5%;
    }

    .content-wrapper {
        display: flex;
        flex-direction: column;
        align-items: center;
        text-align: center;
        max-width: 1000px;
        width: 100%;
        gap: 3.5rem;
    }

    .name-role-group {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 2rem;
    }

    .name {
        font-size: clamp(4.5rem, 10vw, 10rem);
        font-weight: 300;
        color: #1a1a1a;
        line-height: 0.9;
        letter-spacing: -0.02em;
        margin: 0;
        font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    }

    .name-line {
        display: block;
        position: relative;
    }

    .role-section {
        max-width: 600px;
        margin: 0 auto;
    }

    .role {
        font-size: clamp(1.1rem, 1.3vw, 1.3rem);
        font-weight: 400;
        color: #666666;
        line-height: 1.5;
        margin: 0;
        font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
        letter-spacing: 0.02em;
        transition: color 0.3s;
        opacity: 0.85;
    }

    .social-links {
        display: flex;
        align-items: center;
        gap: 1.25rem;
        flex-wrap: wrap;
        justify-content: center;
        margin-top: 0.5rem;
    }

    .social-link {
        text-decoration: none;
        color: inherit;
        transition: all 0.3s ease;
        position: relative;
    }

    .social-text {
        font-size: 0.85rem;
        font-weight: 400;
        color: #666666;
        letter-spacing: 0.03em;
        font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
        transition: all 0.3s ease;
    }

    .social-link:hover .social-text {
        color: #1a1a1a;
        transform: translateY(-1px);
    }


    .scroll-indicator {
        position: absolute;
        bottom: 4rem;
        left: 50%;
        transform: translateX(-50%);
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 0.4rem;
        opacity: 0;
        cursor: pointer;
        transition: all 0.3s ease;
    }

    .scroll-indicator:hover {
        gap: 0.6rem;
    }


    .chevrons {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 2px;
        margin-top: 2px;
    }
    .chevron {
        display: block;
        opacity: 0.6;
        transform: translateY(0);
        animation: chevron-bounce 1.5s infinite;
    }
    .chevron1 {
        animation-delay: 0s;
    }
    .chevron2 {
        animation-delay: 0.2s;
    }
    .chevron3 {
        animation-delay: 0.4s;
    }
    @keyframes chevron-bounce {
        0% { opacity: 0.6; transform: translateY(0); }
        30% { opacity: 1; transform: translateY(6px); }
        60% { opacity: 0.6; transform: translateY(0); }
        100% { opacity: 0.6; transform: translateY(0); }
    }

    /* Responsive adjustments */
    @media (max-width: 768px) {
        .hero {
            padding: 0 8%;
        }
        
        .content-wrapper {
            gap: 4rem;
        }

        .social-links {
            gap: 1.25rem;
        }

        .social-text {
            font-size: 0.85rem;
        }

        .scroll-indicator {
            bottom: 2.5rem;
        }
    }

    @media (max-width: 480px) {
        .hero {
            padding: 0 10%;
        }
        
        .content-wrapper {
            gap: 3.5rem;
        }


        .name {
            line-height: 0.9;
        }

        .social-links {
            gap: 1rem;
        }

        .social-text {
            font-size: 0.8rem;
        }

        .scroll-indicator {
            bottom: 2rem;
        }
    }


</style>
