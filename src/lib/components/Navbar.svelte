<script lang="ts">
    import { gsap } from 'gsap';
    import { ScrollToPlugin } from 'gsap/ScrollToPlugin';
    import { MorphSVGPlugin } from 'gsap/MorphSVGPlugin';
    gsap.registerPlugin(ScrollToPlugin);
    gsap.registerPlugin(MorphSVGPlugin);
    
    let navbar: HTMLElement;
    let menuButton: HTMLElement;
    let menuOverlay: HTMLElement;
    let menuItems: HTMLElement;
    let isMenuOpen = $state(false);
    let navBrand: HTMLElement;
    let brandSvg: SVGSVGElement;
    let brandPath: SVGPathElement;
    let menuIconPath: SVGPathElement;

    // Path data for morphing
    const pathStart = "M9 15H17M1 13L7 7L1 1";
    const pathEnd = "M8 14C9.125 18 15.875 18 17 14M1 13L7 7L1 1M24 13L18 7L24 1";
    const hamburgerPath = "M1 7H17M1 13H17M1 1H17";
    const xPath = "M13 1L1 13M1 1L13 13";

    $effect(() => {
        // Set initial states
        gsap.set(menuOverlay, { opacity: 0, background: 'rgba(0,0,0,0.0)', pointerEvents: 'none', visibility: 'hidden' });
        gsap.set(menuItems, { opacity: 0, y: 20 });
        gsap.set(menuButton, { color: '#1a1a1a' });
        gsap.set(navBrand, { color: '#1a1a1a' });
        if (!menuIconPath) return;
        gsap.to(menuIconPath, {
            duration: 0.4,
            morphSVG: isMenuOpen ? xPath : hamburgerPath,
            ease: 'power2.inOut'
        });
    });

    function toggleMenu() {
        if (!isMenuOpen) {
            isMenuOpen = true;
            openMenu();
        } else {
            closeMenu();
        }
    }

    function openMenu() {
        const menuItemLinks = menuItems.querySelectorAll('.menu-item');
        const menuLines = menuButton.querySelectorAll('.menu-line');
        const tl = gsap.timeline();
        tl.set(menuOverlay, { visibility: "visible", pointerEvents: 'auto' })
          .to(menuOverlay, {
            opacity: 1,
            background: 'rgba(0,0,0,0.95)',
            duration: 0.5,
            ease: 'power2.inOut',
            onStart: () => {
              gsap.to([menuButton, navBrand], {
                color: '#fff',
                duration: 0.3,
                ease: 'power2.inOut'
              });
            }
          })
          .to(menuLines, {
            scaleX: 1.2,
            rotation: 10,
            duration: 0.18,
            ease: 'power1.out',
          }, "-=0.4")
          .to(menuLines, {
            scaleX: 1,
            rotation: 10,
            duration: 0.18,
            ease: 'power1.in',
          })
          .to(menuItems, {
            opacity: 1,
            y: 0,
            duration: 0.3,
            ease: 'power2.out'
          }, "-=0.3")
          .to(menuItemLinks, {
            opacity: 1,
            y: 0,
            color: '#fff',
            duration: 0.5,
            stagger: 0.08,
            ease: "power3.out"
          }, "-=0.2");
    }

    function closeMenu() {
        const menuItemLinks = menuItems.querySelectorAll('.menu-item');
        const tl = gsap.timeline();
        tl.to(menuItemLinks, {
            opacity: 0,
            y: 20,
            color: '#fff',
            duration: 0.28,
            stagger: {
              each: 0.05,
              from: "end"
            },
            ease: "power3.in"
        }, "+=0.03")
        .to(menuItems, {
            opacity: 0,
            y: 20,
            duration: 0.18,
            ease: 'power2.in'
        }, "-=0.12")
        .to(menuOverlay, {
            opacity: 0,
            background: 'rgba(0,0,0,0.0)',
            duration: 0.28,
            ease: 'power2.inOut',
            onComplete: () => {
                gsap.set(menuOverlay, { visibility: "hidden", pointerEvents: 'none' });
                gsap.to([menuButton, navBrand], {
                    color: '#1a1a1a',
                    duration: 0.2,
                    ease: 'power2.inOut'
                });
                isMenuOpen = false;
            }
        }, "+=0");
    }

    function scrollToSection(sectionId: string) {
        const section = document.getElementById(sectionId);
        if (section) {
            gsap.to(window, {
                duration: 1,
                scrollTo: { y: section, offsetY: 0 },
                ease: 'power2.inOut',
                onComplete: () => {
                    closeMenu();
                    isMenuOpen = false;
                }
            });
        }
    }

    function morphTo() {
        if (brandPath) {
            gsap.to(brandPath, {
                duration: 0.6,
                morphSVG: pathEnd,
                ease: 'power2.inOut'
            });
        }
    }

    function morphBack() {
        if (brandPath) {
            gsap.to(brandPath, {
                duration: 0.6,
                morphSVG: pathStart,
                ease: 'power2.inOut'
            });
        }
    }
</script>

<nav class="navbar" bind:this={navbar}>
    <div class="nav-content">
        <div class="nav-brand" bind:this={navBrand} class:inverted={isMenuOpen}>
            <a href="/" class="brand-link" style="text-decoration: none;"
                aria-label="Go to home page"
                onmouseenter={morphTo}
                onmouseleave={morphBack}
            >
                <svg width="25" height="18" viewBox="0 0 25 18" fill="none" xmlns="http://www.w3.org/2000/svg" bind:this={brandSvg}>
                    <path
                        d="M9 15H17M1 13L7 7L1 1"
                        stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"
                        bind:this={brandPath}
                    />
                </svg>
            </a>
        </div>
        
        <button class="menu-button" bind:this={menuButton} class:inverted={isMenuOpen} onclick={toggleMenu} aria-label="Toggle menu">
            <svg width="18" height="14" viewBox="0 0 18 14" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path
                    bind:this={menuIconPath}
                    d="M1 7H17M1 13H17M1 1H17"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                />
            </svg>
        </button>
    </div>

    <div class="menu-overlay" bind:this={menuOverlay} role="dialog" aria-label="Navigation menu" tabindex="-1" onkeydown={(e) => e.key === 'Escape' && (closeMenu())}>
        <div class="menu-content" bind:this={menuItems} onclick={(e) => {e.stopPropagation()}} onkeydown={(e) => e.stopPropagation()} role="menu" tabindex="-1">
            <div class="menu-items">
                <button type="button" class="menu-item" aria-label="Go to Home section" onclick={() => scrollToSection('home')}>
                    <span class="menu-number">01</span>
                    <span class="menu-text">Home</span>
                </button>
                <button type="button" class="menu-item" aria-label="Go to About section" onclick={() => scrollToSection('about')}>
                    <span class="menu-number">02</span>
                    <span class="menu-text">About</span>
                </button>
                <button type="button" class="menu-item" aria-label="Go to Projects section" onclick={() => scrollToSection('projects')}>
                    <span class="menu-number">03</span>
                    <span class="menu-text">Projects</span>
                </button>
                <button type="button" class="menu-item" aria-label="Go to Contact section" onclick={() => scrollToSection('contact')}>
                    <span class="menu-number">04</span>
                    <span class="menu-text">Contact</span>
                </button>
            </div>
        </div>
    </div>
</nav>

<style>
    .navbar {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        z-index: 1000;
        padding: 1.5rem 2rem;
    }

    .nav-content {
        display: flex;
        justify-content: space-between;
        align-items: center;
        max-width: 1200px;
        margin: 0 auto;
    }

    .nav-brand {
        z-index: 1001;
        color: #1a1a1a;
        transition: color 0.3s;
    }

    .nav-brand.inverted {
        color: #fff;
    }

    .brand-link {
        font-size: 1.5rem;
        font-weight: 400;
        color: inherit;
        text-decoration: none;
        letter-spacing: -0.02em;
        font-family: 'Fira Mono', Menlo, Consolas, monospace;
        transition: color 0.3s ease;
        display: flex;
        align-items: center;
        gap: 0.1em;
    }

    .brand-link svg {
        height: 28px;
        width: 28px;
        color: inherit;
        transition: color 0.3s, transform 0.2s cubic-bezier(.4,1.4,.6,1), box-shadow 0.2s;
    }

    .brand-link:hover,
    .brand-link:focus {
        color: #666666;
    }

    .brand-link:hover svg,
    .brand-link:focus svg {
        color: #666666;
    }

    .menu-button {
        position: relative;
        z-index: 1001;
        background: none;
        border: none;
        cursor: pointer;
        width: 30px;
        height: 30px;
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 0;
        color: #1a1a1a;
        transition: color 0.3s;
    }

    .menu-button.inverted {
        color: #fff;
    }

    .menu-button svg {
        width: 18px;
        height: 18px;
        display: block;
    }

    .menu-overlay {
        position: fixed;
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
        background: transparent;
        backdrop-filter: none;
        display: flex;
        justify-content: center;
        align-items: center;
        opacity: 0;
        pointer-events: none;
        visibility: hidden;
        overflow: hidden;
        /* Remove transition for background and opacity, GSAP will handle */
    }

    .menu-overlay.open {
        pointer-events: auto;
        visibility: visible;
    }

    .menu-content {
        text-align: center;
        position: relative;
        z-index: 2;
        color: #fff;
        opacity: 1;
        transition: opacity 0.3s, transform 0.3s;
    }

    .menu-items {
        display: flex;
        flex-direction: column;
        gap: 2rem;
    }

    .menu-item {
        display: flex;
        align-items: center;
        gap: 1rem;
        text-decoration: none;
        color: #fff;
        font-size: clamp(2rem, 4vw, 3rem);
        font-weight: 300;
        letter-spacing: -0.02em;
        font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
        transition: all 0.3s ease;
        padding: 0.5rem 0;
    }

    .menu-item:hover {
        transform: translateX(10px);
    }

    .menu-number {
        font-size: 0.8rem;
        font-weight: 400;
        color: #bbb;
        letter-spacing: 0.1em;
        min-width: 2rem;
    }

    .menu-text {
        position: relative;
    }

    .menu-text::after {
        content: '';
        position: absolute;
        bottom: -5px;
        left: 0;
        width: 0;
        height: 1px;
        background: #fff;
        transition: width 0.3s ease;
    }

    .menu-item:hover .menu-text::after {
        width: 100%;
    }

    /* Responsive adjustments */
    @media (max-width: 768px) {
        .navbar {
            padding: 1rem 1.5rem;
        }

        .menu-items {
            gap: 1.5rem;
        }

        .menu-item {
            font-size: clamp(1.8rem, 5vw, 2.5rem);
        }
    }

    @media (max-width: 480px) {
        .navbar {
            padding: 0.75rem 1rem;
        }

        .menu-items {
            gap: 1.25rem;
        }

        .menu-item {
            font-size: clamp(1.5rem, 6vw, 2rem);
        }
    }
</style> 