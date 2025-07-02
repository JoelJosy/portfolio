<script lang="ts">
import { onMount, onDestroy } from 'svelte';
let circleElement: HTMLDivElement | null = null;

// Mouse and cursor positions
const mouse = { x: 0, y: 0 };
const previousMouse = { x: 0, y: 0 };
const circle = { x: 0, y: 0 };

// Animation state
let currentScale = 0;
let currentAngle = 0;
const speed = 0.17;

function handleMouseMove(e: MouseEvent) {
  mouse.x = e.x;
  mouse.y = e.y;
}

function tick() {
  // MOVE
  circle.x += (mouse.x - circle.x) * speed;
  circle.y += (mouse.y - circle.y) * speed;
  const translateTransform = `translate(${circle.x}px, ${circle.y}px)`;

  // SQUEEZE
  const deltaMouseX = mouse.x - previousMouse.x;
  const deltaMouseY = mouse.y - previousMouse.y;
  previousMouse.x = mouse.x;
  previousMouse.y = mouse.y;
  const mouseVelocity = Math.min(Math.sqrt(deltaMouseX**2 + deltaMouseY**2) * 4, 150);
  const scaleValue = (mouseVelocity / 150) * 0.5;
  currentScale += (scaleValue - currentScale) * speed;
  const scaleTransform = `scale(${1 + currentScale}, ${1 - currentScale})`;

  // ROTATE
  const angle = Math.atan2(deltaMouseY, deltaMouseX) * 180 / Math.PI;
  if (mouseVelocity > 20) {
    currentAngle = angle;
  }
  const rotateTransform = `rotate(${currentAngle}deg)`;

  // Apply transforms
  if (circleElement) {
    circleElement.style.transform = `${translateTransform} ${rotateTransform} ${scaleTransform}`;
  }
  window.requestAnimationFrame(tick);
}

onMount(() => {
  window.addEventListener('mousemove', handleMouseMove);
  window.requestAnimationFrame(tick);
  return () => {
    window.removeEventListener('mousemove', handleMouseMove);
  };
});
</script>

<style>
.squishy-circle {
  --circle-size: 40px;
  position: fixed;
  height: var(--circle-size);
  width: var(--circle-size);
  border: 1px solid #b7b7b7;
  border-radius: 100%;
  top: calc(var(--circle-size) / 2 * -1);
  left: calc(var(--circle-size) / 2 * -1);
  pointer-events: none;
  z-index: 9999;
  background: transparent;
  transition: border-color 0.2s;
  will-change: transform;
}

@media (pointer: coarse), (max-width: 768px) {
  .squishy-circle {
    display: none;
  }
}
</style>

<div bind:this={circleElement} class="squishy-circle"></div> 