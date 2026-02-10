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
    large: 'span-6',
    medium: 'span-4',
    small: 'span-3'
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
    max-width: 1400px;
    margin: auto;
  }

  .field {
    display: grid;
    grid-template-columns: repeat(12, 1fr);
    gap: clamp(1.5rem, 4vw, 4rem);
  }

  figure {
    margin: 0;
  }

  img {
    width: 100%;
    height: auto;
    display: block;
    border-radius: 2px;
  }
/* BASE IMAGE RESET */
.ratio img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

  .span-6 { grid-column: span 6; }
  .span-4 { grid-column: span 4; }
  .span-3 { grid-column: span 3; }

  /* DESKTOP: enforce horizontal feel */
@media (min-width: 901px) {

  .ratio {
    width: 100%;
    aspect-ratio: 4 / 3;
    overflow: hidden;
  }
  .span-6 .ratio { aspect-ratio: 5 / 3; }
  .span-4 .ratio { aspect-ratio: 4 / 3; }
  .span-3 .ratio { aspect-ratio: 3 / 2; }

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
