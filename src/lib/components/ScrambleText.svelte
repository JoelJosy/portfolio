<script lang="ts">
  import { onMount } from 'svelte';
  import { gsap } from 'gsap';

  export let text = "Joel";
  export let altText = "Josy";
  let el: HTMLSpanElement;
  let currentText = text;

  // More coder-y character set
  const chars = "$%#@!_[]{}<>|/\\";

  // Slower scramble effect
  function scramble(revealText: string, duration = 2.5) {
    let frame = 0;
    const totalFrames = Math.floor(60 * duration);
    let scrambled = "";

    gsap.ticker.remove(update);
    function update() {
      scrambled = "";
      for (let i = 0; i < revealText.length; i++) {
        if (frame > i * 3) {
          scrambled += revealText[i];
        } else {
          scrambled += chars[Math.floor(Math.random() * chars.length)];
        }
      }
      el.textContent = scrambled;
      frame++;
      if (frame > revealText.length * 3) {
        el.textContent = revealText;
        gsap.ticker.remove(update);
        currentText = revealText;
      }
    }
    frame = 0;
    gsap.ticker.add(update);
  }

  onMount(() => {
    scramble(text);
  });
</script>

<span
  bind:this={el}
  onmouseenter={() => scramble(altText)}
  onmouseleave={() => scramble(text)}
  style="font-family: 'Fira Mono', 'Menlo', 'Consolas', monospace; font-size: 1.5rem; font-weight: 400; letter-spacing: -0.02em; cursor: pointer; transition: color 0.3s;"
>
  {text}
</span> 