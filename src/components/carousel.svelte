<script>
  import { onMount, createEventDispatcher } from 'svelte';
  import { tweened } from 'svelte/motion';
  import { cubicOut } from 'svelte/easing';

  export let items = [];
  export let autoplay = true;
  export let interval = 5000;
  export let transitionDuration = 500;

  function goToSlide(index) {
    currentIndex = index;
    dispatch('change', { index: currentIndex });
  }

  onMount(() => {
    if (autoplay) {
      const intervalId = setInterval(nextSlide, interval);
      return () => clearInterval(intervalId);
    }
  });
</script>

<div class="carousel-container" bind:this={carousel} bind:clientWidth={containerWidth}>
  <div class="carousel-track" style="transform: translateX({$offset}px)">
    {#each items as item, index}
      <div class="carousel-slide">
        <img src={item.image} alt={item.alt || `Slide ${index + 1}`} />
        {#if item.caption}
          <div class="caption">{item.caption}</div>
        {/if}
      </div>
    {/each}
  </div>

  <button class="carousel-control prev" on:click={prevSlide} aria-label="Previous slide">
    &lt;
  </button>
  <button class="carousel-control next" on:click={nextSlide} aria-label="Next slide">
    &gt;
  </button>

  <div class="carousel-indicators">
    {#each items as _, index}
      <button
        class="indicator"
        class:active={currentIndex === index}
        on:click={() => goToSlide(index)}
        aria-label={`Go to slide ${index + 1}`}
      ></button>
    {/each}
  </div>
</div>

<style>
  .carousel-container {
    position: relative;
    width: 100%;
    overflow: hidden;
  }

  .carousel-track {
    display: flex;
    transition: transform 0.5s ease-out;
  }

  .carousel-slide {
    flex: 0 0 100%;
    position: relative;
  }

  .carousel-slide img {
    width: 100%;
    height: auto;
  }

</style>
