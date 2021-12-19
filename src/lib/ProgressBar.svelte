<script lang="ts">
  export let percent = 0
  export let nextSection = false
  export let moving
</script>

<div class="progress-bar" style={`--percent: ${percent}`}>
  {#if nextSection && moving}
    <div class="highlight" />
  {/if}
</div>

<style>
  .progress-bar {
    position: relative;
    background: rgba(255, 255, 255, 0.5);
    height: 0.5rem;
    flex: 1;
  }

  .highlight {
    width: 100%;
    height: 100%;
    position: absolute;
    background: rgba(255, 255, 127, 0.2);
  }

  .progress-bar:not(:last-of-type) {
    margin-right: 0.2rem;
  }

  .progress-bar::after {
    display: block;
    height: 100%;
    background: rgba(255, 255, 255, 1);
    content: '';
    transform-origin: center left;
    transform: scaleX(var(--percent));
    z-index: 1;
  }
</style>
