<script lang="ts">
  export let playlistId: string;
  export let height: number = 380;
</script>

<section class="spotify-hero" style="background-image: url('./images/spc.jpg');">
  <div class="overlay">
    <div class="embed">
      <iframe
        src={`https://open.spotify.com/embed/playlist/${playlistId}?utm_source=generator`}
        width="100%"
        height={height}
        frameborder="0"
        allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture"
        loading="lazy"
      ></iframe>
    </div>
  </div>
</section>

<style>
  .spotify-hero {
    position: relative;
    min-height: 100vh;
    
    background-size: cover;
    background-position: center;
    background-attachment: fixed; /* subtle parallax feel */
    display: grid;
    place-items: center;
    padding: 2rem;
  }

  /* subtle dark overlay for readability */
  .spotify-hero::before {
    content: "";
    position: absolute;
    inset: 0;
    background: rgba(0,0,0,0.35);
  }

  .overlay {
    position: relative;
    z-index: 1;
    text-align: center;
    max-width: 600px;
    width: 100%;
  }



  .embed {
    border-radius: 16px;
    overflow: hidden;
    box-shadow: 0 40px 80px rgba(0,0,0,0.4);
  }

  iframe {
    display: block;
    border: none;
  }

  @media (max-width: 900px) {
    .spotify-hero {
      background-attachment: scroll; /* better mobile performance */
      min-height: 80vh;
    }
  }
</style>