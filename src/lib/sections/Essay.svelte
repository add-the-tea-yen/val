<script lang="ts">
  type Placement = 'wide' | 'float-left' | 'float-right' | 'normal';

  interface TextBlock {
    type: 'text';
    content: string;
    placement?: Placement;
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
      <p
        class:dropcap={i === 0}
        class={block.placement ?? 'normal'}
      >
        {block.content}
      </p>

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
  margin-bottom: 0.8rem;
  line-height: 1.1;
}

article {
  max-width: 820px;
  margin: 0 auto;
  font-size: clamp(1.15rem, 1.2vw, 1.35rem);
  line-height: 1.75;

  /* MAGAZINE COLUMNS */
  column-width: 360px;
  column-gap: 4rem;
}

/* TEXT */
p {
  margin-bottom: 1.5rem;
  break-inside: avoid;
  margin: 0 0 1.4rem 0;
}

/* DROP CAP */
.dropcap::first-letter {
  float: left;
  font-size: 4.2rem;
  line-height: 0.9;
  padding-right: 0.6rem;
  font-weight: 600;
}

/* IMAGES */
figure {
  margin: 1.8rem 0;
  break-inside: avoid;
}

figure img {
  width: 100%;
  height: auto;
  display: block;
}

/* WIDE IMAGE — SPANS ALL COLUMNS */
figure.wide {
  column-span: all;
  width: 100%;
  margin: 1rem 0;
}

/* FLOATS DISABLED INSIDE COLUMNS */
.float-left,
.float-right {
  float: none;
  width: 100%;
  margin: 1rem 0;
}

/* MOBILE */
@media (max-width: 700px) {
  article {
    column-width: auto;
  }

  .dropcap::first-letter {
    float: none;
    font-size: 2.5rem;
    padding-right: 0.2rem;
  }
}

p.float-left {
  float: left;
  width: 45%;
  margin: 0 2rem 1.5rem 0;
}

p.float-right {
  float: right;
  width: 45%;
  margin: 0 0 1.5rem 2rem;
}
p.wide {
  column-span: all;
  font-size: 1.2rem;
  line-height: 1.8;
  margin: 1rem 0;
}
article::after {
  content: "";
  display: block;
  clear: both;
}


</style>
