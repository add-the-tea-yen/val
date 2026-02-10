<script lang="ts">
  type Placement = 'wide' | 'float-left' | 'float-right';

  interface TextBlock {
    type: 'text';
    content: string;
  }

  interface ImageBlock {
    type: 'image';
    src: string;
    alt: string;
    placement: Placement;
  }

  type EssayBlock = TextBlock | ImageBlock;

  interface EssayData {
    title: string;
    paragraphs: EssayBlock[];
  }

  export let data: EssayData;
</script>

<section class="essay">
  <h1>{data.title}</h1>

  <article>
    {#each data.paragraphs as block, i}
      {#if block.type === 'text'}
        <p class:dropcap={i === 0}>{block.content}</p>
      {:else}
        <figure class={block.placement}>
          <img src={block.src} alt={block.alt} loading="lazy" />
        </figure>
      {/if}
    {/each}
  </article>
</section>
<style>
  section {
    scroll-margin-top: 0px;
    padding: clamp(4rem, 10vw, 8rem) 1.5rem;
    max-width: 1100px;
    margin: auto;
  }

  h1 {
    font-size: clamp(2rem, 5vw, 3rem);
    font-weight: 600;
    margin-bottom: 3rem;
    line-height: 1.1;
  }

  article {
    max-width: 720px;
    margin: auto;
    font-size: 1.05rem;
    line-height: 1.7;
  }

  p {
    margin-bottom: 1.5rem;
  }

  
  /* DROP CAP */
  .dropcap::first-letter {
    float: left;
    font-size: 3.8rem;
    line-height: 1;
    padding-right: 0.5rem;
    font-weight: 600;
  }

  /* IMAGES */
  figure {
    margin: 3rem 0;
  }

  figure img {
    width: 100%;
    height: auto;
    display: block;
  }

  /* PLACEMENTS */
  .wide {
    max-width: 100%;
  }

  .float-right {
    float: right;
    width: 45%;
    margin: 0 0 1.5rem 2rem;
  }

  .float-left {
    float: left;
    width: 45%;
    margin: 0 2rem 1.5rem 0;
  }

  /* CLEAR FLOATS */
  article::after {
    content: "";
    display: block;
    clear: both;
  }




  /* MOBILE RESET */
  @media (max-width: 700px) {
    .float-right,
    .float-left {
      float: none;
      width: 100%;
      margin: 2rem 0;
    }

    .dropcap::first-letter {
      float: none;
      font-size: 2.5rem;
      padding-right: 0.2rem;
    }
  }

@media (min-width: 1000px) {
  article {
    max-width: 1000px;
    column-width: 360px;
    column-gap: 4rem;
  }

  figure.wide {
    column-span: all;
    width: 100%;
    margin: 4rem 0;
  }

  .float-left,
  .float-right {
    width: 100%;
    margin: 2rem 0;
  }
}

p,
figure {
  break-inside: avoid;
}

</style>
