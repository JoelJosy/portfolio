<script lang="ts">
    import { gsap } from 'gsap';
    
    let navbar: HTMLElement;
    let menuButton: HTMLElement;
    let menuOverlay: HTMLElement;
    let menuItems: HTMLElement;
    let isMenuOpen = $state(false);

    $effect(() => {
        // Set initial states with better starting positions
        gsap.set(menuOverlay, { opacity: 0, scale: 0.98 });
        gsap.set(menuItems, { opacity: 0, y: 20 });
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
        // Create a timeline for coordinated animations
        const tl = gsap.timeline();
        
        // Show overlay with smooth scale and fade
        tl.set(menuOverlay, { visibility: "visible" })
          .to(menuOverlay, {
            opacity: 1,
            scale: 1,
            duration: 0.5,
            ease: "power2.out"
        });

        // Animate menu items with staggered entrance
        tl.to(menuItems, {
            opacity: 1,
            y: 0,
            duration: 0.7,
            ease: "power3.out"
        }, "-=0.2");

        // Animate menu button with smooth rotation
        tl.to(menuButton, {
            rotation: 45,
            duration: 0.4,
            ease: "power2.out"
        }, "-=0.5");
    }

    function closeMenu() {
        // Create a timeline for coordinated animations
        const tl = gsap.timeline();
        
        // Animate menu button first
        tl.to(menuButton, {
            rotation: 0,
            duration: 0.3,
            ease: "power2.out"
        });

        // Hide menu items
        tl.to(menuItems, {
            opacity: 0,
            y: 20,
            duration: 0.4,
            ease: "power3.in"
        }, "-=0.1");

        // Hide overlay with smooth scale and fade
        tl.to(menuOverlay, {
            opacity: 0,
            scale: 0.98,
            duration: 0.4,
            ease: "power2.in"
        }, "-=0.2")
        .set(menuOverlay, { visibility: "hidden" });
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
        <div class="nav-brand">
            <a href="/" class="brand-link">Joel J.</a>
        </div>
        
        <button class="menu-button" bind:this={menuButton} onclick={toggleMenu} aria-label="Toggle menu">
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
    }

    .brand-link {
        font-size: 1.5rem;
        font-weight: 400;
        color: #1a1a1a;
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
    }

    .menu-line {
        width: 20px;
        height: 2px;
        background: #1a1a1a;
        transition: all 0.3s ease;
        transform-origin: center;
    }

    .menu-overlay {
        position: fixed;
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
        background: rgba(255, 255, 255, 0.98);
        backdrop-filter: blur(10px);
        display: flex;
        justify-content: center;
        align-items: center;
        opacity: 0;
        pointer-events: none;
        visibility: hidden;
    }

    .menu-overlay.open {
        pointer-events: auto;
        visibility: visible;
    }

    .menu-content {
        text-align: center;
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
        color: #1a1a1a;
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
        color: #999999;
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
        background: #1a1a1a;
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