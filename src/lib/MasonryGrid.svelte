<script>
  import { onMount, onDestroy } from "svelte";

  let observer;

  function resize(tile) {
    const rowHeight = 10;
    const height = tile.getBoundingClientRect().height;
    tile.style.gridRowEnd = `span ${Math.ceil(height / rowHeight)}`;
  }

  onMount(() => {
    observer = new ResizeObserver(entries => {
      for (const entry of entries) {
        resize(entry.target);
      }
    });

    document
      .querySelectorAll(".masonry-tile.auto")
      .forEach(el => observer.observe(el));
  });

  onDestroy(() => observer?.disconnect());
</script>

<div class="masonry">
  <slot />
</div>

<style>
  .masonry {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
    grid-auto-rows: 10px;
    gap: 16px;
  }
</style>
