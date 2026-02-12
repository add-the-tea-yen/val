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

<section class="book-strip">
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
    font-size: clamp(1.5rem, 4vw, 2rem);
    margin-bottom: 2.5rem;
    font-weight: 500;
  }

  .row {
    display: flex;
    gap: clamp(1.5rem, 3vw, 3rem);
    overflow-x: auto;
    scroll-behavior: smooth;
    padding-bottom: 1rem;
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
    flex: 0 0 auto;
    width: 200px;           /* fixed width */
    height: 300px;          /* fixed height */
    display: block;
    position: relative;
    transition: transform 0.25s ease;
    aspect-ratio: 2 / 3;
  }

.book img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  background: transparent;      /* keeps proportions visually consistent */
  border-radius: 4px;
  display: block;
}

  .book:hover {
    transform: translateY(-8px);
  }

  @media (min-width: 1200px) {
    .book {
      width: 220px;
    }
  }
</style>