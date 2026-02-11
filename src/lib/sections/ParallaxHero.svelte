<script lang="ts">
  import { onMount } from 'svelte';

  export let src: string;
  export let alt: string = '';
  export let height: string = '100vh'; // customizable

  let section: HTMLElement;
  let offset = 0;

  function handleScroll() {
    const rect = section.getBoundingClientRect();
    const speed = 0.4; // parallax intensity (lower = subtler)
    offset = rect.top * speed;
  }

  onMount(() => {
    handleScroll();
    window.addEventListener('scroll', handleScroll);
    return () => window.removeEventListener('scroll', handleScroll);
  });
</script>

<section
  bind:this={section}
  class="parallax"
  style="height: {height};"
>
  <div
    class="image"
    style="transform: translateY({offset}px); background-image: url('{src}');"
    aria-label={alt}
  />
</section>

<style>
  .parallax {
    position: relative;
    overflow: hidden;
    width: 100%;
  }

  .image {
    position: absolute;
    inset: -10% 0; /* extra height prevents white gaps */
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    will-change: transform;
  }

  @media (max-width: 900px) {
    .parallax {
      height: 70vh;
    }
  }
</style>
