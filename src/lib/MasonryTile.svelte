<script>
  export let item;
</script>

<div
  class="masonry-tile {item.type} {item.rows ? 'fixed' : 'auto'}"
  style={item.rows ? `grid-row: span ${item.rows}` : ""}
>
  {#if item.type === "image"}
    <img src={item.src} alt={item.title} loading="lazy" />

  {:else if item.type === "essay"}
    <div class="essay">
      <h3>{item.title}</h3>
      {#each item.content as paragraph}
        <p>{paragraph}</p>
      {/each}
    </div>

  {:else if item.type === "youtube"}
    <iframe
      src={`https://www.youtube.com/embed/${item.videoId}`}
      allowfullscreen
    ></iframe>

  {:else if item.type === "spotify"}
    <iframe
      src={`https://open.spotify.com/embed/${item.embed}`}
      allow="encrypted-media"
    ></iframe>
  {/if}
</div>

<style>
  .masonry-tile {
    border-radius: 14px;
    overflow: hidden;
    background: #111;
    box-shadow: 0 6px 20px rgba(0,0,0,0.25);
  }

  img {
    width: 100%;
    height: auto;
    display: block;
  }

  iframe {
    width: 100%;
    height: 100%;
    border: none;
  }

  .youtube {
    height: 200px;
  }

  .spotify {
    height: 152px;
  }

  .essay {
    padding: 16px;
  }
</style>
