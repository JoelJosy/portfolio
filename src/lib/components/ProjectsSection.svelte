<script lang="ts">
import { tick } from 'svelte';
import { gsap } from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

interface Project {
  title: string;
  tech: string;
  description: string;
  link: string;
}

const projects: Project[] = [
  {
    title: 'Project 1',
    tech: 'Node.js, Express.js',
    description: 'A RESTful API server built with Node.js and Express.js, providing robust endpoints for a scalable web application.',
    link: 'https://example.com/project1',
  },
  {
    title: 'Project 2',
    tech: 'React, TypeScript',
    description: 'A modern single-page application using React and TypeScript, featuring dynamic UI and state management.',
    link: 'https://example.com/project2',
  },
  {
    title: 'Project 3',
    tech: 'Python, Flask',
    description: 'A lightweight web app built with Flask, focusing on rapid prototyping and clean RESTful APIs.',
    link: 'https://example.com/project3',
  },
  {
    title: 'Project 4',
    tech: 'Svelte, Vite',
    description: 'A blazing fast Svelte app bundled with Vite, demonstrating reactive UI and smooth transitions.',
    link: 'https://example.com/project4',
  },
  {
    title: 'Project 5',
    tech: 'Next.js, MongoDB',
    description: 'A full-stack project using Next.js and MongoDB, featuring server-side rendering and a NoSQL database.',
    link: 'https://example.com/project5',
  },
];

let hoveredIndex = $state<number | null>(null);
let cardEl = $state<HTMLDivElement | null>(null);
let cardContentEl = $state<HTMLDivElement | null>(null);
let prevIndex = $state<number | null>(null);
let projectsListEl = $state<HTMLDivElement | null>(null);
let projectsTitleEl: HTMLElement;

let cardX = $state(0);
let cardY = $state(0);
let cardVisible = $state(false);
let quickToX: any = null;
let quickToY: any = null;
let latestMouseX = $state(0);
let latestMouseY = $state(0);
let isRevealing = $state(false);
let pendingRevealIdx = $state<number | null>(null);
let isExiting = $state(false);
let isAnimating = $state(false);

$effect(() => {
  if (!projectsTitleEl || !projectsListEl) return;
  
  // Animate title on scroll
  gsap.fromTo(
    projectsTitleEl,
    { y: 60, opacity: 0 },
    {
      y: 0,
      opacity: 1,
      duration: 1.2,
      ease: "power3.out",
      scrollTrigger: {
        trigger: projectsListEl,
        start: 'top 80%',
        end: 'top 20%',
        scrub: false,
        markers: false,
        toggleActions: 'play none none reverse'
      }
    }
  );

  // Animate project rows with stagger
  const projectRows = projectsListEl.querySelectorAll('.project-row');
  gsap.fromTo(
    projectRows,
    { y: 40, opacity: 0 },
    {
      y: 0,
      opacity: 1,
      stagger: 0.15,
      duration: 1.0,
      ease: "power2.out",
      scrollTrigger: {
        trigger: projectsListEl,
        start: 'top 70%',
        end: 'bottom 100%',
        scrub: false,
        markers: false,
        toggleActions: 'play none none reverse'
      }
    }
  );
});

function handleRowClick(idx: number) {
  window.open(projects[idx].link, '_blank');
}

async function handleMouseMove(e: MouseEvent) {
  cardX = e.clientX + 24;
  cardY = e.clientY - 20;
  await tick();
  if (cardEl && cardVisible && !isExiting) {
    if (isRevealing || isAnimating) {
    } else {
      if (quickToX && quickToY) {
        quickToX(cardX);
        quickToY(cardY);
      }
    }
  }
}

function handleMouseLeave() {
  if (cardEl && cardVisible && !isExiting) {
    isExiting = true;
    gsap.to(cardEl, {
      y: cardY + 20,
      opacity: 0,
      duration: 0.28,
      ease: 'power2.in',
      onComplete: () => {
        isExiting = false;
        cardVisible = false;
        hoveredIndex = null;
      }
    });
  } else {
    return;
  }
}

$effect(() => {
  const updateMouse = (e: MouseEvent) => {
    latestMouseX = e.clientX;
    latestMouseY = e.clientY;
  };
  window.addEventListener('mousemove', updateMouse);
  return () => {
    window.removeEventListener('mousemove', updateMouse);
  };
});

async function handleRowEnter(idx: number, e?: MouseEvent) {
  if (!cardVisible) {
    if (e) {
      cardX = e.clientX + 24;
      cardY = e.clientY - 20;
    } else {
      cardX = latestMouseX + 24;
      cardY = latestMouseY - 20;
    }
    cardVisible = true;
    hoveredIndex = idx;
    prevIndex = idx;
    pendingRevealIdx = idx;
  } else if (hoveredIndex !== idx) {
    // Animate content out, then in
    if (cardContentEl) {
      await gsap.to(cardContentEl, { y: 30, opacity: 0, duration: 0.18, ease: 'power1.in' });
    }
    hoveredIndex = idx;
    await tick();
    if (cardContentEl) {
      gsap.fromTo(cardContentEl, { y: -30, opacity: 0 }, { y: 0, opacity: 1, duration: 0.22, ease: 'power2.out' });
    }
    prevIndex = idx;
  }
}

$effect(() => {
  if (pendingRevealIdx !== null && cardEl) {
    isRevealing = true;
    isAnimating = true;
    gsap.set(cardEl, { x: cardX, y: cardY + 20, opacity: 0 });
    gsap.to(cardEl, {
      y: cardY,
      opacity: 1,
      duration: 0.36,
      ease: 'power2.out',
      onUpdate: () => {
        if (cardEl) gsap.set(cardEl, { x: cardX });
      },
      onComplete: () => {
        if (cardEl) {
          quickToX = gsap.quickTo(cardEl, 'x', { duration: 0.32, ease: 'power2.out' });
          quickToY = gsap.quickTo(cardEl, 'y', { duration: 0.32, ease: 'power2.out' });
        }
        isRevealing = false;
        isAnimating = false;
      }
    });
    pendingRevealIdx = null;
  }
});
</script>

<section class="projects-section" id="projects">
  <h2 class="projects-title" bind:this={projectsTitleEl}>Projects</h2>
  <div
    class="projects-list"
    bind:this={projectsListEl}
    onmousemove={handleMouseMove}
    onmouseleave={handleMouseLeave}
    role="list"
  >
    {#each projects as project, idx}
      <div
        class="project-row"
        role="button"
        aria-label={project.title}
        tabindex="0"
        onmouseenter={(e) => handleRowEnter(idx, e)}
        onclick={() => handleRowClick(idx)}
        onkeydown={(e) => { if (e.key === 'Enter' || e.key === ' ') handleRowClick(idx); }}
        style="cursor: pointer;"
      >
        <div class="project-title">{project.title}</div>
        <div class="project-tech">{project.tech}</div>
      </div>
    {/each}
  </div>

  {#if (cardVisible && hoveredIndex !== null) || isExiting}
    <div
      class="project-card"
      bind:this={cardEl}
      style="pointer-events: none; opacity: 1; display: flex;"
    >
      {#if hoveredIndex !== null}
        <div class="card-content" bind:this={cardContentEl}>
          <div class="card-title">{projects[hoveredIndex].title}</div>
          <div class="card-desc">{projects[hoveredIndex].description}</div>
        </div>
      {/if}
    </div>
  {/if}
</section>

<style>
.projects-section {
  width: 100vw;
  padding: 6rem 0 4rem 0;
  background: #fff;
  display: flex;
  flex-direction: column;
  align-items: center;
  box-sizing: border-box;
}
.projects-title {
  font-size: clamp(3rem, 8vw, 6rem);
  font-weight: 200;
  color: #1a1a1a;
  margin-bottom: 4rem;
  letter-spacing: -0.04em;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  line-height: 0.9;
  position: relative;
}
.projects-title::after {
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
.projects-list {
  width: 100%;
  max-width: 1000px;
  margin-left: auto;
  margin-right: auto;
  display: flex;
  flex-direction: column;
  gap: 0;
  border-top: 1px solid #e5e5e5;
  border-bottom: 1px solid #e5e5e5;
}
.project-row {
  width: 100%;
  max-width: 1000px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 2.5rem 4vw;
  background: none;
  border-radius: 0;
  box-shadow: none;
  border: none;
  border-bottom: 1px solid #e5e5e5;
  transition: background 0.2s;
}
.project-row:hover, .project-row:focus {
  background: #f7f7fa;
}
.project-row:last-child {
  border-bottom: none;
}
.project-title {
  font-size: clamp(1.1rem, 1.7vw, 1.4rem);
  font-weight: 500;
  color: #222;
  letter-spacing: -0.01em;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
}
.project-tech {
  font-size: clamp(0.95rem, 1.2vw, 1.1rem);
  font-weight: 400;
  color: #666;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  text-align: right;
}
.project-card {
  position: fixed;
  left: 0;
  top: 0;
  width: 420px;
  height: 280px;
  background: #000000;
  border-radius: 1.2rem;
  box-shadow: 0 20px 60px 0 rgba(0,0,0,0.25), 0 8px 24px 0 rgba(0,0,0,0.15);
  padding: 2.5rem;
  z-index: 1000;
  pointer-events: none;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: center;
  will-change: opacity, transform;
  transform: translate(0, 0);
}
.card-content {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 1.5rem;
}
.card-title {
  font-size: 1.8rem;
  font-weight: 400;
  color: #ffffff;
  letter-spacing: -0.01em;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  line-height: 1.2;
  margin: 0;
}
.card-desc {
  font-size: 1.1rem;
  color: #cccccc;
  line-height: 1.6;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  letter-spacing: 0.02em;
  margin: 0;
}
@media (max-width: 900px) {
  .projects-title {
    font-size: clamp(2.5rem, 10vw, 4rem);
    margin-bottom: 3rem;
  }
  .projects-list, .project-row {
    max-width: 90vw;
  }
  .project-row {
    flex-direction: column;
    align-items: flex-start;
    gap: 1.2rem;
    padding: 2rem 4vw;
  }
  .project-tech {
    text-align: left;
  }
  .project-card {
    width: 360px;
    height: 240px;
    padding: 2rem;
  }
  .card-title {
    font-size: 1.5rem;
  }
  .card-desc {
    font-size: 1rem;
  }
}
@media (max-width: 600px) {
  .projects-section {
    padding: 7rem 0 2.5rem 0;
  }
  .projects-title {
    font-size: clamp(2rem, 12vw, 3rem);
    margin-bottom: 2.5rem;
  }
  .projects-list, .project-row {
    max-width: 95vw;
  }
  .project-row {
    padding: 1.2rem 2vw;
  }
  .project-card {
    width: 300px;
    height: 200px;
    padding: 1.5rem;
  }
  .card-title {
    font-size: 1.3rem;
  }
  .card-desc {
    font-size: 0.95rem;
  }
}

:global(html) {
  scroll-padding-top: 7rem;
}
</style> 