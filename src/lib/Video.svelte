<script lang="ts">
  export let videoElement: HTMLVideoElement = null
  export let src: string = null
  export let show = false
  export let onVideoEnd

  let duration
  let currentTime = 0

  export let percentComplete
  $: percentComplete = currentTime / duration
</script>

<video bind:this={videoElement} bind:duration bind:currentTime class:show on:ended={onVideoEnd}>
  <source {src} type="video/mp4" />
  <track kind="captions" />
</video>

<style>
  video {
    width: 100%;
    height: 100%;
    position: absolute;
  }

  .show {
    z-index: 1;
  }
</style>
