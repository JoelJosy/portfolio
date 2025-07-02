<script lang="ts">

  let currentTime = $state('');
  let footerEl: HTMLElement;
  let timeEl: HTMLElement;

  function updateTime() {
    const now = new Date();
    const timeString = now.toLocaleTimeString('en-US', {
      hour: '2-digit',
      minute: '2-digit',
      hour12: true
    });
    currentTime = timeString;
  }

  $effect(() => {
    updateTime();
    const interval = setInterval(updateTime, 1000);

    return () => clearInterval(interval);
  });
</script>

<footer class="footer" bind:this={footerEl}>
  <div class="footer-content">
    <span class="year">© {new Date().getFullYear()}</span>
    <span class="separator">•</span>
    <span class="time" bind:this={timeEl}>{currentTime}</span>
  </div>
</footer>

<style>
.footer {
  width: 100vw;
  background: #fff;
  padding: 0.5rem 0;
  display: flex;
  justify-content: center;
  align-items: center;
  border-top: 1px solid #f0f0f0;
}
.footer-content {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
}
.year {
  font-size: 0.65rem;
  font-weight: 400;
  color: #666666;
  letter-spacing: 0.02em;
}
.separator {
  font-size: 0.65rem;
  color: #cccccc;
  font-weight: 300;
}
.time {
  font-size: 0.65rem;
  font-weight: 400;
  color: #666666;
  letter-spacing: 0.02em;
  font-family: 'SF Mono', Monaco, 'Cascadia Code', 'Roboto Mono', Consolas, 'Courier New', monospace;
}
@media (max-width: 600px) {
  .footer {
    padding: 0.4rem 0;
  }
  .footer-content {
    gap: 0.4rem;
  }
  .year, .time {
    font-size: 0.6rem;
  }
  .separator {
    font-size: 0.6rem;
  }
}
</style> 