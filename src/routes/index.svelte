<script lang="ts">
  const TIMEOUT = 3000

  const videos = 2
  const videoPlayers = Array(videos).fill(null)

  let currentVideoIndex = 0
  let nextVideoIndex = 0
  let currentVideo: HTMLVideoElement | null = null
  let started = false
  let moving = false
  let timer

  function start() {
    videoPlayers[0].play()
    started = true
  }

  function onVideoEnd() {
    if (moving) {
      nextVideoIndex++
      fetchNextVideo(nextVideoIndex + 1)
    }

    currentVideo = videoPlayers[nextVideoIndex] ?? videoPlayers[videoPlayers.length - 1]
    currentVideoIndex = nextVideoIndex
    currentVideo.play()
  }

  function fetchNextVideo(i) {
    fetch(`/${i}.mp4`)
  }

  function onMove() {
    moving = true
    clearTimeout(timer)

    timer = setTimeout(() => {
      moving = false
      nextVideoIndex = 0
    }, TIMEOUT)
  }

  fetchNextVideo(1)
</script>

<svelte:body on:mousemove={onMove} on:touchmove={onMove} />

<div class="page-wrapper">
  <div class="video-wrapper">
    {#each videoPlayers as _, i}
      <video bind:this={videoPlayers[i]} class:show={i == currentVideoIndex} on:ended={onVideoEnd}>
        <source src={`/${i}.mp4`} type="video/mp4" />
      </video>
    {/each}
  </div>
  {#if !started}
    <button on:click={start}> start </button>
  {/if}
</div>

<style>
  :global(html) {
    margin: 0;
    padding: 0;
  }

  :global(body) {
    margin: 0;
    padding: 0;
  }

  .page-wrapper {
    width: 100vw;
    height: 100vh;
    display: flex;
    flex-direction: column;
    background: black;
  }

  .video-wrapper {
    position: relative;
    aspect-ratio: 16/9;
  }

  video {
    width: 100%;
    height: 100%;
    position: absolute;
  }

  button {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    margin: 1rem auto;
    padding: 4rem;
    background: none;
    border: none;
    color: white;
    font-size: 2rem;
    font-family: monospace;
    z-index: 10;
    cursor: pointer;
  }

  .show {
    z-index: 1;
  }
</style>
