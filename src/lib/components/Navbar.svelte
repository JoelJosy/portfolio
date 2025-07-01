<script lang="ts">
    import { gsap } from 'gsap';
    
    let navbar: HTMLElement;
    let menuButton: HTMLElement;
    let menuOverlay: HTMLElement;
    let menuItems: HTMLElement;
    let menuWipe: HTMLElement;
    let isMenuOpen = $state(false);
    let navBrand: HTMLElement;

    $effect(() => {
        gsap.set(menuOverlay, { opacity: 1, background: 'transparent' });
        gsap.set(menuWipe, { y: '-100%', background: '#000' });
        gsap.set(menuItems, { opacity: 0, y: 20 });
        gsap.set(menuButton, { color: isMenuOpen ? '#fff' : '#1a1a1a' });
        gsap.set(navBrand, { color: isMenuOpen ? '#fff' : '#1a1a1a' });
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
        const tl = gsap.timeline();
        tl.set(menuOverlay, { visibility: "visible" })
          .to(menuWipe, {
            y: '0%',
            duration: 0.6,
            ease: 'power2.inOut',
            onStart: () => {
              menuOverlay.classList.add('inverted');
            }
          })
          .to(menuItems, {
            opacity: 1,
            y: 0,
            duration: 0.32,
            ease: "power2.out"
          }, "-=0.18")
          .to(menuButton, {
            rotation: 45,
            duration: 0.22,
            ease: "power2.out"
          }, "-=0.22");
    }

    function closeMenu() {
        const tl = gsap.timeline();
        tl.to(menuButton, {
            rotation: 0,
            duration: 0.18,
            ease: "power2.inOut"
        });
        tl.to(menuItems, {
            opacity: 0,
            y: 20,
            duration: 0.22,
            ease: "power2.in"
        }, "-=0.12");
        tl.to(menuWipe, {
            y: '-100%',
            duration: 0.5,
            ease: 'power2.inOut',
            onComplete: () => {
              menuOverlay.classList.remove('inverted');
            }
        }, "+=0.01");
        tl.set(menuOverlay, { visibility: "hidden" });
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
        <div class="nav-brand" class:inverted={isMenuOpen} bind:this={navBrand}>
            <a href="/" class="brand-link">J. J.</a>
        </div>
        
        <button class="menu-button" class:inverted={isMenuOpen} bind:this={menuButton} onclick={toggleMenu} aria-label="Toggle menu">
            <span class="menu-line"></span>
            <span class="menu-line"></span>
            <span class="menu-line"></span>
        </button>
    </div>

    <div class="menu-overlay" class:open={isMenuOpen} bind:this={menuOverlay} onclick={handleNavClick} onkeydown={(e) => e.key === 'Escape' && handleNavClick()} role="dialog" aria-label="Navigation menu" tabindex="-1">
        <div class="menu-wipe" bind:this={menuWipe}></div>
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
        z-index: 1100;
        padding: 1.5rem 2rem;
    }

    .nav-content {
        display: flex;
        justify-content: space-between;
        align-items: center;
        max-width: 1200px;
        margin: 0 auto;
        position: relative;
        z-index: 1200;
    }

    .nav-brand {
        z-index: 1201;
        color: #1a1a1a;
        transition: color 0.3s;
    }

    .nav-brand.inverted {
        color: #fff !important;
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
        z-index: 1201;
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
        color: #fff !important;
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
        display: flex;
        justify-content: center;
        align-items: center;
        opacity: 1;
        pointer-events: none;
        visibility: hidden;
        overflow: hidden;
        z-index: 1001;
        transition: none;
    }

    .menu-overlay.open {
        pointer-events: auto;
        visibility: visible;
    }

    .menu-overlay.inverted {
        /* Used to trigger color inversion for overlay and icons */
    }

    .menu-wipe {
        position: absolute;
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
        background: #000;
        z-index: 1;
        transform: translateY(-100%);
        transition: none;
    }

    .menu-content {
        text-align: center;
        position: relative;
        z-index: 2;
        color: #fff;
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