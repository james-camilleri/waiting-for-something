<script lang="ts">
  import ProgressBar from '$lib/ProgressBar.svelte'
  import Video from '$lib/Video.svelte'

  const TIMEOUT = 500
  const TOTAL_VIDEOS = 4

  interface Video {
    videoElement: HTMLVideoElement
    percentComplete: number
  }

  let videos: Video[] = Array(TOTAL_VIDEOS).fill(null)
  videos = videos.map(() => ({
    videoElement: null,
    percentComplete: 0
  }))

  let currentVideoIndex = 0
  let nextVideoIndex = 0
  let currentVideo: Video | null = null
  let started = false
  let moving = false
  let timer

  function start() {
    videos[0].videoElement.play()
    started = true
  }

  function onVideoEnd() {
    // Do nothing if we've reached the last video.
    if (currentVideoIndex == TOTAL_VIDEOS - 1) return

    if (moving) {
      nextVideoIndex++
      fetchNextVideo(nextVideoIndex + 1)
    } else {
      // Video has been reset, clear all progress bars.
      videos.forEach((video) => (video.percentComplete = 0))
    }

    currentVideo = videos[nextVideoIndex] ?? videos[videos.length - 1]
    currentVideoIndex = nextVideoIndex
    currentVideo.videoElement.play()
  }

  function fetchNextVideo(i) {
    // fetch(`/${i}.mp4`)
  }

  function onMove() {
    moving = started && true
    clearTimeout(timer)

    timer = setTimeout(() => {
      moving = false
      nextVideoIndex = 0
    }, TIMEOUT)
  }

  fetchNextVideo(1)
</script>

<svelte:body on:mousemove={onMove} on:touchmove={onMove} on:click={onMove} on:scroll={onMove} />

<div class="page-wrapper">
  <div class="progress-bars">
    {#each videos as { percentComplete }, i}
      <ProgressBar percent={percentComplete} nextSection={i === nextVideoIndex + 1} {moving} />
    {/each}
  </div>
  <div class="video-wrapper">
    {#each videos as video, i}
      <Video
        src={`/${i}.mp4`}
        show={i == currentVideoIndex}
        bind:percentComplete={video.percentComplete}
        bind:videoElement={video.videoElement}
        {onVideoEnd}
      />
    {/each}
  </div>
  {#if !started}
    <button on:click={start}> start </button>
    <p>(fidget with your mouse to advance the soundscape)</p>
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

  .progress-bars {
    display: flex;
  }

  .video-wrapper {
    position: relative;
    aspect-ratio: 16/9;
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

  p {
    font-family: monospace;
    color: white;
    text-align: center;
  }
</style>
