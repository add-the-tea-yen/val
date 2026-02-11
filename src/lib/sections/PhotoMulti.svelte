<script lang="ts">
  type PhotoSize = 'small' | 'medium' | 'large';

  interface PhotoItem {
    src: string;
    alt: string;
    size: PhotoSize;
  }

  interface PhotoMultiData {
    title?: string;
    photos: PhotoItem[];
  }
  function isPhotoSize(value: string): value is PhotoSize {
  return value === 'small' || value === 'medium' || value === 'large';
  }


  export let data: {
  title?: string;
  photos: { src: string; alt: string; size: string }[];
};

const normalized: PhotoMultiData = {
  title: data.title,
  photos: data.photos.map((p) => ({
    ...p,
    size: isPhotoSize(p.size) ? p.size : 'medium'
  }))
};


  const layouts: Record<PhotoSize, string> = {
  large: 'span-6 row-2',
  medium: 'span-4 row-2',
  small: 'span-3 row-1'
};

</script>

<section class="photo-multi">
<div class="field">
  {#each normalized.photos as photo}
    <figure class={layouts[photo.size]}>
  <div class="ratio">
    <img src={photo.src} alt={photo.alt} loading="lazy" />
  </div>
</figure>
  {/each}
</div>
</section>

<style>
  section {
    padding: clamp(4rem, 10vw, 8rem) 1.5rem;
    max-width: 1700px; /* widened */
    margin: auto;
}


  .field {
    display: grid;
    grid-template-columns: repeat(12, 1fr);
    gap: clamp(1.5rem, 3vw, 3rem);

    align-items: start; /* prevents vertical stretching */
    grid-auto-rows: 160px;
    grid-auto-flow: dense;
}


  figure {
    margin: 0;
    height: 100%;
  }

  img {
    width: 100%;
    height: auto;
    object-fit: cover;
    display: block;
    border-radius: 2px;
  }
.ratio {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #ffffff; /* subtle fill for empty space */
}

.ratio img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  display: block;
}


  .span-6 { grid-column: span 6; }
  .span-4 { grid-column: span 4; }
  .span-3 { grid-column: span 3; }

  .row-1 { grid-row: span 1; }
  .row-2 { grid-row: span 2; }
  .row-3 { grid-row: span 3; }


  /* DESKTOP: enforce horizontal feel */
@media (min-width: 901px) {

  .span-6 { grid-column: span 7; }
  .span-4 { grid-column: span 5; }
  .span-3 { grid-column: span 4; }

}


  @media (max-width: 600px) {
    .field {
      grid-template-columns: repeat(4, 1fr);
      gap: 1.25rem;
    }

    figure {
      grid-column: span 4;
    }
  }
</style>
