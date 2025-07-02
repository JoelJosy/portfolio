<script lang="ts">
  import { gsap } from 'gsap';
  import { ScrollTrigger } from 'gsap/ScrollTrigger';
  import SplitType from 'split-type';

  gsap.registerPlugin(ScrollTrigger);

  let aboutText: HTMLElement;
  let sectionEl: HTMLElement;
  let titleEl: HTMLElement;

  $effect(() => {
    if (!aboutText || !titleEl) return;
    
    // Animate title on scroll
    gsap.fromTo(
      titleEl,
      { y: 60, opacity: 0 },
      {
        y: 0,
        opacity: 1,
        duration: 1.2,
        ease: "power3.out",
        scrollTrigger: {
          trigger: sectionEl,
          start: 'top 50%',
          end: 'bottom 100%',
          scrub: false,
          markers: false,
          toggleActions: 'play none none reverse'
        }
      }
    );


    const split = new SplitType(aboutText, { types: 'chars' });
    gsap.fromTo(
      split.chars,
      { color: '#eaeaea', y: 20 },
      {
        color: '#1a1a1a',
        y: 0,
        stagger: 0.03,
        duration: 1.5,
        ease: "power2.out",
        scrollTrigger: {
          trigger: sectionEl,
          start: 'top 60%',
          end: 'bottom 100%',
          scrub: true,
          markers: false,
          toggleActions: 'play play reverse reverse'
        }
      }
    );
  });
</script>

<section class="about-section" bind:this={sectionEl} id="about">
  <div class="about-content">
    <h2 class="about-title" bind:this={titleEl}>About Me</h2>
    <p class="about-text" bind:this={aboutText} style="color:#eaeaea;">
      Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
    </p>
  </div>
</section>

<style>
.about-section {
  width: 100vw;
  height: 100vh;
  background: #fff;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: relative;
  overflow: hidden;
}
.about-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  max-width: 800px;
  padding: 0 5%;
  text-align: center;
}
.about-title {
  font-size: clamp(3rem, 8vw, 6rem);
  font-weight: 200;
  color: #1a1a1a;
  margin: 0 0 4rem 0;
  letter-spacing: -0.04em;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  line-height: 0.9;
  position: relative;
}
.about-title::after {
  content: '';
  position: absolute;
  bottom: -1rem;
  left: 50%;
  transform: translateX(-50%);
  width: 3rem;
  height: 1px;
  background: #e5e5e5;
  opacity: 0.6;
}
.about-text {
  font-size: clamp(1.2rem, 2.5vw, 1.8rem);
  font-weight: 300;
  color: #1a1a1a;
  line-height: 1.8;
  letter-spacing: 0.02em;
  margin: 0;
  max-width: 100%;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  transition: color 0.3s;
  opacity: 0.85;
}
@media (max-width: 900px) {
  .about-content {
    max-width: 90vw;
  }
  .about-title {
    font-size: clamp(2.5rem, 10vw, 4rem);
    margin-bottom: 3rem;
  }
  .about-text {
    font-size: clamp(1.1rem, 4vw, 1.4rem);
    line-height: 1.7;
  }
}
@media (max-width: 600px) {
  .about-content {
    max-width: 95vw;
  }
  .about-title {
    font-size: clamp(2rem, 12vw, 3rem);
    margin-bottom: 2.5rem;
  }
  .about-text {
    font-size: clamp(1rem, 5vw, 1.2rem);
    line-height: 1.6;
  }
}
</style> 