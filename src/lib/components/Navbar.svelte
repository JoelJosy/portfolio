<script lang="ts">
    import { gsap } from 'gsap';
    
    let navbar: HTMLElement;
    let menuButton: HTMLElement;
    let menuOverlay: HTMLElement;
    let menuItems: HTMLElement;
    let isMenuOpen = $state(false);
    let navBrand: HTMLElement;

    $effect(() => {
        // Set initial states
        gsap.set(menuOverlay, { opacity: 0, background: 'rgba(0,0,0,0.0)', pointerEvents: 'none', visibility: 'hidden' });
        gsap.set(menuItems, { opacity: 0, y: 20 });
        gsap.set(menuButton, { color: '#1a1a1a' });
        gsap.set(navBrand, { color: '#1a1a1a' });
    });

    function toggleMenu() {
        isMenuOpen = !isMenuOpen;
        
        if (isMenuOpen) {
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
            rotation: 20,
            duration: 0.18,
            ease: 'power1.out',
          }, "-=0.4")
          .to(menuLines, {
            scaleX: 1,
            rotation: 20,
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
        const menuLines = menuButton.querySelectorAll('.menu-line');
        const tl = gsap.timeline();
        tl.to(menuLines, {
            scaleX: 0.85,
            rotation: 0,
            duration: 0.15,
            ease: 'power1.in',
        })
        .to(menuLines, {
            scaleX: 1,
            rotation: 0,
            duration: 0.15,
            ease: 'power1.out',
        })
        .to(menuItemLinks, {
            opacity: 0,
            y: 20,
            color: '#fff',
            duration: 0.3,
            stagger: {
              each: 0.06,
              from: "end"
            },
            ease: "power3.in"
        }, "-=0.2")
        .to(menuItems, {
            opacity: 0,
            y: 20,
            duration: 0.3,
            ease: 'power2.in'
        }, "<")
        .to(menuOverlay, {
            opacity: 0,
            background: 'rgba(0,0,0,0.0)',
            duration: 0.4,
            ease: 'power2.inOut',
            onComplete: () => {
                gsap.set(menuOverlay, { visibility: "hidden", pointerEvents: 'none' });
                gsap.to([menuButton, navBrand], {
                    color: '#1a1a1a',
                    duration: 0.3,
                    ease: 'power2.inOut'
                });
            }
        }, "-=0.2");
    }

    function handleNavClick() {
        if (isMenuOpen) {
            closeMenu();
            isMenuOpen = false;
        }
    }
</script>

<nav class="navbar" bind:this={navbar}>
    <div class="nav-content">
        <div class="nav-brand" bind:this={navBrand} class:inverted={isMenuOpen}>
            <a href="/" class="brand-link">J. J.</a>
        </div>
        
        <button class="menu-button" bind:this={menuButton} class:inverted={isMenuOpen} onclick={toggleMenu} aria-label="Toggle menu">
            <span class="menu-line"></span>
            <span class="menu-line"></span>
            <span class="menu-line"></span>
        </button>
    </div>

    <div class="menu-overlay" class:open={isMenuOpen} bind:this={menuOverlay} onclick={handleNavClick} onkeydown={(e) => e.key === 'Escape' && handleNavClick()} role="dialog" aria-label="Navigation menu" tabindex="-1">
        <div class="menu-content" bind:this={menuItems} onclick={(e) => {e.stopPropagation()}} onkeydown={(e) => e.stopPropagation()} role="menu" tabindex="-1">
            <div class="menu-items">
                <a href="/" class="menu-item" onclick={handleNavClick}>
                    <span class="menu-number">01</span>
                    <span class="menu-text">Home</span>
                </a>
                <a href="/projects" class="menu-item" onclick={handleNavClick}>
                    <span class="menu-number">02</span>
                    <span class="menu-text">Projects</span>
                </a>
                <a href="/about" class="menu-item" onclick={handleNavClick}>
                    <span class="menu-number">03</span>
                    <span class="menu-text">About</span>
                </a>
                <a href="/contact" class="menu-item" onclick={handleNavClick}>
                    <span class="menu-number">04</span>
                    <span class="menu-text">Contact</span>
                </a>
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
        font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
        transition: color 0.3s ease;
    }

    .brand-link:hover {
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
        flex-direction: column;
        justify-content: center;
        align-items: center;
        gap: 4px;
        padding: 0;
        color: #1a1a1a;
        transition: color 0.3s;
    }

    .menu-button.inverted {
        color: #fff;
    }

    .menu-line {
        width: 20px;
        height: 2px;
        background: currentColor;
        transition: all 0.3s ease;
        transform-origin: center;
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