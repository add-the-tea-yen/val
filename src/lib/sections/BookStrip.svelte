<script lang="ts">
  export interface Book {
    title: string;
    cover: string;
    pdf: string;
  }

  export interface BookStripData {
    title: string;
    books: Book[];
  }

  export let data: BookStripData;
</script>

<section class="book-strip" style="overflow-x: hidden;">
  <h2>{data.title}</h2>

  <div class="row">
    {#each data.books as book}
      <a
        class="book"
        href={book.pdf}
        target="_blank"
        rel="noopener noreferrer"
        aria-label={`Open ${book.title}`}
      >
        <img src={book.cover} alt={book.title} loading="lazy" />
      </a>
    {/each}
  </div>
</section>

<style>
  section {
    padding: clamp(4rem, 10vw, 8rem) 1.5rem;
    max-width: 1700px;
    margin: auto;
  }

  h2 {
    font-family: 'Helvetica Neue',sans-serif;
    font-size: clamp(1.5rem, 4vw, 2rem);
    margin-bottom: 2.5rem;
    font-weight: 500;
    color: #1f3a6f;
  }

  .row {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: clamp(1.2rem, 3vw, 2.5rem);

  max-width: 900px;
  margin: 0 auto;
  background: transparent;
}

  /* Hide scrollbar cleanly */
  .row::-webkit-scrollbar {
    display: none;
  }
  .row {
    -ms-overflow-style: none;
    scrollbar-width: none;
  }

.book {
  width: 100%;
  max-width: 220px;
  aspect-ratio: 2 / 3;

  margin: 0 auto;
  display: block;
  position: relative;
  overflow: hidden;

  transition:
    transform 0.25s ease,
    box-shadow 0.25s ease;

  box-shadow: 0 2px 6px rgba(0,0,0,0.06);
  background: transparent;
}


.book img {

  width: 100%;
  height: auto;
  background: transparent;      /* keeps proportions visually consistent */
  border-radius: 4px;
  display: block;
}

  .book:hover {
  transform: translateY(-6px) scale(1.015);
  box-shadow:
    0 12px 24px rgba(0,0,0,0.16),
    0 2px 6px rgba(0,0,0,0.08);
  background:transparent ;
}




  @media (min-width: 1200px) {
    .book {
      width: 220px;
    }
  }
 @media (max-width: 900px) {
  .row {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 600px) {
  .row {
    grid-template-columns: 1fr;
  }
}

</style>