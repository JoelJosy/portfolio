<script lang="ts">
  import { gsap } from 'gsap';
  import SplitType from 'split-type';

  let line0: HTMLElement;
  let line1: HTMLElement;
  let line2: HTMLElement;
  let sectionEl: HTMLElement;

  const textData = [
    "A peek into the things I've been creating —",
    "practical, fun, personal,",
    "and everything in between."
  ];

  $effect(() => {
    (async () => {
      const pkg = await import('gsap/ScrollTrigger');
      const { ScrollTrigger } = pkg;
      gsap.registerPlugin(ScrollTrigger);

      const lineRefs = [line0, line1, line2];
      const splitLines = lineRefs.map((el) => new SplitType(el, { types: 'chars' }));

      const tl = gsap.timeline({
        scrollTrigger: {
          trigger: sectionEl,
          start: 'top 40%',
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
          { color: '#eaeaea', y: 20 },
          {
            color: '#1a1a1a',
            y: 0,
            stagger: 0.03,
            duration: 1.5,
            ease: "power2.out"
          },
          `${(i * 100) / lineCount}%` 
        );
      });
    })();
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