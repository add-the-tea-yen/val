<section class="hero">
  <div class="frame">
    <img src="/images/hero/cover.jpg" alt="cover"/>
  </div>
</section>

<style>
  .hero {
    min-height: 100vh;
    background: #ffd400; /* NatGeo yellow */
    display: grid;
    place-items: center;
    padding: clamp(2rem, 6vw, 5rem);
  }

  /* IMAGE FRAME */
  .frame {
    aspect-ratio: 3 / 4.5; /* tall magazine cover */
    max-height: 85vh;
    width: auto;
  }

  .frame img {
    height: 100%;
    width: auto;
    max-width: 100%;
    display: block;
    object-fit: contain;
  }

  /* MOBILE TUNING */
  @media (max-width: 600px) {
    .frame {
      max-height: 75vh;
      aspect-ratio: 3 / 4;
    }
  }
</style>
