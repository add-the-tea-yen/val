<script>
  import MasonryGrid from "$lib/MasonryGrid.svelte";
  import MasonryTile from "$lib/MasonryTile.svelte";
  import media from "$lib/content/media.json";
</script>

<MasonryGrid>
  {#each media as item (item.id)}
    <MasonryTile {item} />
  {/each}
</MasonryGrid>
