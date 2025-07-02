<script lang="ts">
  import { gsap } from 'gsap';
  import { ScrollTrigger } from 'gsap/ScrollTrigger';
  import SplitType from 'split-type';
  gsap.registerPlugin(ScrollTrigger);

  let contactText: HTMLElement;
  let sectionEl: HTMLElement;
  let titleEl: HTMLElement;

  $effect(() => {
    if (!contactText || !titleEl) return;
    
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
          start: 'top 70%',
          end: 'top 20%',
          scrub: false,
          markers: false,
          toggleActions: 'play none none reverse'
        }
      }
    );

    // Animate text with character reveal
    const split = new SplitType(contactText, { types: 'chars' });
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

  function handleEmailClick() {
    window.open('mailto:joeljosy79@gmail.com', '_blank');
  }
</script>

<section class="contact-section" bind:this={sectionEl} id="contact">
  <div class="contact-content">
    <h2 class="contact-title" bind:this={titleEl}>Let's Talk</h2>
    <p class="contact-text" bind:this={contactText} style="color:#eaeaea;">
      I'm always open to new opportunities and interesting conversations. Feel free to reach out.
    </p>
    
    <div class="contact-actions">
      <button class="email-button" onclick={handleEmailClick}>
        <span class="email-text">Send Email</span>
      </button>
      
      <div class="social-links">
        <a href="https://github.com/JoelJosy" class="social-link" aria-label="GitHub">
          <span class="social-text">GitHub</span>
        </a>
        <a href="https://linkedin.com/in/joel-josy" class="social-link" aria-label="LinkedIn">
          <span class="social-text">LinkedIn</span>
        </a>
        <a href="https://twitter.com/joeljosy" class="social-link" aria-label="Twitter">
          <span class="social-text">Twitter</span>
        </a>
      </div>
    </div>
  </div>
</section>

<style>
.contact-section {
  width: 100vw;
  min-height: 80vh;
  background: #fff;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: relative;
  overflow: hidden;
  padding: 4rem 0;
}
.contact-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  max-width: 800px;
  padding: 0 5%;
  text-align: center;
}
.contact-title {
  font-size: clamp(3rem, 8vw, 6rem);
  font-weight: 200;
  color: #1a1a1a;
  margin: 0 0 4rem 0;
  letter-spacing: -0.04em;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  line-height: 0.9;
  position: relative;
}
.contact-title::after {
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
.contact-text {
  font-size: clamp(1.2rem, 2.5vw, 1.8rem);
  font-weight: 300;
  color: #1a1a1a;
  line-height: 1.8;
  letter-spacing: 0.02em;
  margin: 0 0 4rem 0;
  max-width: 100%;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  transition: color 0.3s;
  opacity: 0.85;
}
.contact-actions {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 3rem;
}
.email-button {
  background: none;
  border: 1px solid #1a1a1a;
  padding: 1.2rem 2.5rem;
  cursor: pointer;
  transition: all 0.3s ease;
  border-radius: 0;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
}
.email-button:hover {
  background: #1a1a1a;
  color: #fff;
}
.email-text {
  font-size: 1rem;
  font-weight: 400;
  letter-spacing: 0.02em;
}
.social-links {
  display: flex;
  align-items: center;
  gap: 2rem;
  flex-wrap: wrap;
  justify-content: center;
}
.social-link {
  text-decoration: none;
  color: inherit;
  transition: all 0.3s ease;
  position: relative;
}
.social-text {
  font-size: 0.9rem;
  font-weight: 400;
  color: #666666;
  letter-spacing: 0.03em;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  transition: all 0.3s ease;
}
.social-link:hover .social-text {
  color: #1a1a1a;
  transform: translateY(-1px);
}
@media (max-width: 900px) {
  .contact-content {
    max-width: 90vw;
  }
  .contact-title {
    font-size: clamp(2.5rem, 10vw, 4rem);
    margin-bottom: 3rem;
  }
  .contact-text {
    font-size: clamp(1.1rem, 4vw, 1.4rem);
    line-height: 1.7;
    margin-bottom: 3rem;
  }
  .contact-actions {
    gap: 2.5rem;
  }
  .social-links {
    gap: 1.5rem;
  }
}
@media (max-width: 600px) {
  .contact-content {
    max-width: 95vw;
  }
  .contact-title {
    font-size: clamp(2rem, 12vw, 3rem);
    margin-bottom: 2.5rem;
  }
  .contact-text {
    font-size: clamp(1rem, 5vw, 1.2rem);
    line-height: 1.6;
    margin-bottom: 2.5rem;
  }
  .contact-actions {
    gap: 2rem;
  }
  .email-button {
    padding: 1rem 2rem;
  }
  .social-links {
    gap: 1.25rem;
  }
  .social-text {
    font-size: 0.85rem;
  }
}
</style> 