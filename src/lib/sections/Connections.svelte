<svelte:head>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link
    href="https://fonts.googleapis.com/css2?family=Libre+Franklin:wght@500;600;700&display=swap"
    rel="stylesheet"
  />
</svelte:head>


<script>
  const MAX_MISTAKES = 4;

  let mistakes = MAX_MISTAKES;
  let selected = new Set();
  let solvedGroups = new Set();
  let oneAway = false;
  let feedbackTimeout;
  
  let words = [
    { text: "NET", group: 0 },
    { text: "RINK", group: 1 },
    { text: "MOON", group: 2 },
    { text: "CAKE", group: 2 },

    { text: "RETURN", group: 0 },
    { text: "PUCK", group: 1 },
    { text: "BLOCK", group: 0 },
    { text: "GAIN", group: 0 },

    { text: "PINE", group: 2 },
    { text: "BRICK", group: 3 },
    { text: "COASTER", group: 3 },
    { text: "BAG", group: 3 },

    { text: "DERBY", group: 1 },
    { text: "SWOON", group: 2 },
    { text: "YIELD", group: 0 },
    { text: "YEARN", group: 2 }
  ];

  function toggle(word) {
    if (solvedGroups.has(word.group)) return;

    if (selected.has(word)) selected.delete(word);
    else if (selected.size < 4) selected.add(word);

    selected = new Set(selected);
  }

function submit() {
  if (selected.size !== 4) return;

  const chosen = [...selected];
  const groups = chosen.map(w => w.group);

  // Count how many of each group were selected
  const counts = {};
  for (const g of groups) {
    counts[g] = (counts[g] || 0) + 1;
  }

  const maxMatch = Math.max(...Object.values(counts));

  if (maxMatch === 4) {
    // Correct group
    solvedGroups.add(groups[0]);
    oneAway = false;
  } else if (maxMatch === 3) {
    // One away
    oneAway = true;

    clearTimeout(feedbackTimeout);
    feedbackTimeout = setTimeout(() => {
      oneAway = false;
    }, 1500);
  } else {
    // Wrong guess
    mistakes--;
    oneAway = false;
  }

  selected.clear();
  selected = new Set(selected);
}



  function shuffle() {
    words = [...words].sort(() => Math.random() - 0.5);
  }


  function deselectAll() {
    selected.clear();
    selected = new Set(selected);
    oneAway = false;
  }
</script>
<section id="connections">
  <h2>Create four groups of four!</h2>

  <div class="grid">
    {#each words as word}
      <button
        class:selected={selected.has(word)}
        class:solved={solvedGroups.has(word.group)}
        on:click={() => toggle(word)}
      >
        {word.text}
      </button>
    {/each}
  </div>

{#if oneAway}
  <div class="one-away">One away…</div>
{/if}

  <div class="status">
    <span>Mistakes Remaining:</span>
    {#each Array(mistakes) as _}
      <span class="dot"></span>
    {/each}
  </div>

  <div class="controls">
    <button on:click={shuffle}>Shuffle</button>
    <button on:click={deselectAll}>Deselect All</button>
    <button on:click={submit} disabled={selected.size !== 4}>
      Submit
    </button>
  </div>
</section>
<style>
  section {
    font-family:
      "Libre Franklin",
      "Franklin Gothic Medium",
      "Helvetica Neue",
      Helvetica,
      Arial,
      sans-serif;

    -webkit-font-smoothing: antialiased;
    text-rendering: optimizeLegibility;

    padding: clamp(1.5rem, 5vw, 4rem) 0.75rem;
    max-width: min(96vw, 720px);
    margin: auto;
    text-align: center;
    background-color: white;
  }

  h2 {
    margin-bottom: 1.25rem;
    font-weight: 600;
    letter-spacing: 0.01em;
    font-size: clamp(1rem, 3.5vw, 1.4rem);
  }

  /* LOCKED 4-COLUMN GRID */
  .grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: clamp(0.35rem, 1.5vw, 0.75rem);
    margin-bottom: 1.5rem;
  }

  button {
    aspect-ratio: 1 / 1;               /* square tiles */
    padding: clamp(0.4rem, 2vw, 1rem);
    border-radius: 10px;
    border: none;
    background: #f0efe6;
    font-family: inherit;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.045em;
    line-height: 1;
    font-size: clamp(0.62rem, 2.8vw, 0.9rem);
    cursor: pointer;
    transition: background 0.2s ease, transform 0.15s ease;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
    word-break: break-word;
  }

  button:active {
    transform: scale(0.96);
  }

  button.selected {
    background: #bdbdbd;
  }

  button.solved {
    background: #8bc34a;
    color: white;
    cursor: default;
  }

  /* STATUS */
  .status {
    margin-bottom: 1.25rem;
    font-size: clamp(0.75rem, 2.5vw, 0.9rem);
  }

  .dot {
    display: inline-block;
    width: clamp(7px, 2vw, 9px);
    height: clamp(7px, 2vw, 9px);
    background: #555;
    border-radius: 50%;
    margin: 0 3px;
  }

  /* CONTROLS */
  .controls {
    display: flex;
    justify-content: center;
    gap: 0.5rem;
    flex-wrap: wrap;
  }

.status,
  .controls button {
    font-weight: 500;
    letter-spacing: 0.01em;
  }

  .controls button {
    padding: 0.5rem 1rem;
    font-size: clamp(0.7rem, 2.5vw, 0.85rem);
  }
.one-away {
  margin-bottom: 1rem;
  font-size: clamp(0.8rem, 2.5vw, 0.95rem);
  font-weight: 600;
  letter-spacing: 0.02em;
}

</style>
