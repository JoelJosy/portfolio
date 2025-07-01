<script lang="ts">
  import { onMount } from 'svelte';
  import { gsap } from 'gsap';
  import { ScrollTrigger } from 'gsap/ScrollTrigger';
  import SplitType from 'split-type';

  gsap.registerPlugin(ScrollTrigger);

  let line0: HTMLElement;
  let line1: HTMLElement;
  let line2: HTMLElement;
  let sectionEl: HTMLElement;

  // Split the provided text into three visually balanced lines
  const textData = [
    "A peek into the things I've been creating —",
    "practical, fun, personal,",
    "and everything in between"
  ];

  onMount(() => {
    const lineRefs = [line0, line1, line2];
    const splitLines = lineRefs.map((el) => new SplitType(el, { types: 'chars' }));

    const tl = gsap.timeline({
      scrollTrigger: {
        trigger: sectionEl,
        start: 'top 50%',
        end: 'bottom 100%',
        scrub: true,
        markers: false,
        toggleActions: 'play play reverse reverse'
      }
    });

    const lineCount = splitLines.length;
    splitLines.forEach((split, i) => {
      tl.fromTo(
        split.chars,
        { color: '#eaeaea' },
        {
          color: '#1a1a1a',
          stagger: 0.02,
          duration: 1 // duration is controlled by scroll, but needed for GSAP
        },
        `${(i * 100) / lineCount}%` // Start each line at 0%, 33%, 66%
      );
    });
  });
</script>

<section class="animated-section" bind:this={sectionEl}>
  <div class="lines-wrapper">
    <p class="reveal-type" bind:this={line0} style="color:#eaeaea;">{textData[0]}</p>
    <p class="reveal-type" bind:this={line1} style="color:#eaeaea;">{textData[1]}</p>
    <p class="reveal-type" bind:this={line2} style="color:#eaeaea;">{textData[2]}</p>
  </div>
</section>

<style>
.animated-section {
  width: 100vw;
  min-height: 100vh;
  background: #fff;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 0 5%;
}
.lines-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem;
  width: 100%;
  max-width: 1000px;
}
.reveal-type {
  font-size: clamp(1.5rem, 4vw, 3rem);
  font-weight: 300;
  color: #1a1a1a;
  line-height: 1.1;
  letter-spacing: -0.02em;
  margin: 0;
  text-align: center;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  transition: color 0.3s;
}
@media (max-width: 768px) {
  .animated-section {
    padding: 0 8%;
  }
  .lines-wrapper {
    gap: 1.5rem;
  }
  .reveal-type {
    font-size: clamp(1.1rem, 6vw, 2rem);
  }
}
@media (max-width: 480px) {
  .animated-section {
    padding: 0 10%;
  }
  .lines-wrapper {
    gap: 1rem;
  }
  .reveal-type {
    font-size: clamp(1rem, 7vw, 1.3rem);
  }
}
</style> 