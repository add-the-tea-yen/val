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
  let solvedGroups = [];
  let solvedRows = [];
  let oneAway = false;
  let feedbackTimeout;

  const groupColors = {
  0: "#f9df6d", // yellow
  1: "#a0c35a", // green
  2: "#b0c4ef", // blue
  3: "#c59ad9"  // purple
};
const categories = {
  0: { name: "Hockey Terms", color: "#f9df6d" },
  1: { name: "Longing", color: "#a0c35a" },
  2: { name: "___ Cake", color: "#b0c4ef" },
  3: { name: "Things You Put Drinks On", color: "#f5a6c8" }
};

const GROUP_INFO = {
  0: {
    label: "CONSTRUCT",
    words: ["FORM", "MAKE", "MOLD", "PRODUCE"]
  },
  1: {
    label: "HOCKEY TERMS",
    words: ["RINK", "PUCK", "DERBY", "ICE"]
  },
  2: {
    label: "LONGING",
    words: ["MOON", "PINE", "YEARN", "SWOON"]
  },
  3: {
    label: "THINGS YOU PUT DRINKS ON",
    words: ["COASTER", "COUNTER", "TRAY", "TABLE"]
  }
};

let words = [
  { text: "NET", group: 0 },
  { text: "RETURN", group: 0 },
  { text: "BLOCK", group: 0 },
  { text: "YIELD", group: 0 },

  { text: "RINK", group: 1 },
  { text: "PUCK", group: 1 },
  { text: "DERBY", group: 1 },
  { text: "ICE", group: 1 },

  { text: "MOON", group: 2 },
  { text: "CAKE", group: 2 },
  { text: "PINE", group: 2 },
  { text: "YEARN", group: 2 },

  { text: "BRICK", group: 3 },
  { text: "COASTER", group: 3 },
  { text: "BAG", group: 3 },
  { text: "COUNTER", group: 3 }
];


  function toggle(word) {
    if (solvedGroups.some(g => g.id === word.group)) return;

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
  const group = groups[0];

  solvedGroups = [
    ...solvedGroups,
    {
      id: group,
      label: GROUP_INFO[group].label,
      words: GROUP_INFO[group].words
    }
  ];

  oneAway = false;
}

else if (maxMatch === 3) {
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
  <div class="game">
    <h1 style="font-style: italic;">Connections</h1>
  <h2>Create four groups of four!</h2>
  
  {#each solvedGroups as group}
  <div class="solved-row group-{group.id}">
    <div class="solved-category">
      {group.label}
    </div>

    <div class="solved-words">
      {group.words.join(', ')}
    </div>
  </div>
{/each}


  <div class="grid">
    {#each words.filter(w => !solvedGroups.some(g => g.id === w.group)) as word}
      <button
        class:selected={selected.has(word)}
        class:solved={solvedGroups.some(g => g.id === word.group)}
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
    
    min-height: 100vh;
    display: flex;
    align-items: center;            /* vertical center */
    justify-content: center;

    padding: clamp(1.5rem, 5vw, 4rem) 0.75rem;
    max-width: min(96vw, 720px);
    margin: auto;
    text-align: center;
    background-color: white;
    
  }
.game {
  width: 100%;
  max-width: 520px;

  display: flex;
  flex-direction: column;
  align-items: center;

  justify-content: center; /* keeps grid centered even after solving */
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
    min-height: 0px;
    grid-auto-rows: clamp(70px, 10vw, 110px);
    transition: min-height 0.2s ease;
  }

  button {
  border-radius: 12px;
  border: none;
  background: #e7e5dd;
  font-weight: 700;
  letter-spacing: 0.05em;
  text-transform: uppercase;
  cursor: pointer;
  transition: background 0.2s ease, transform 0.1s ease;
}
.grid button {
padding: clamp(0.55rem, 1.5vw, 0.7rem);
  min-height: clamp(52px, 6vw, 60px);
  border-radius: 8px;
  border: none;
  background: #efeee7;
  font-family: inherit;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.04em;
  line-height: 1.1;
  font-size: clamp(0.7rem, 2.2vw, 0.9rem);
  cursor: pointer;
  transition: background 0.2s ease, transform 0.15s ease;

  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  transition: 
    background 0.15s ease,
    transform 0.08s ease,
    box-shadow 0.12s ease;

  box-shadow: inset 0 0 0 1px #e1dfd6;
}

.grid button:hover {
  background: #e3e1d7;
}
.grid button:active {
  transform: scale(0.97);
  box-shadow: inset 0 0 0 1px #d5d2c8;
}
.grid button.selected {
  background: #5a5a57;
  color: white;
  box-shadow: none;
}
.grid button.solved {
  color: #1c1c1c;
  box-shadow: none;
}

button:hover {
  background: #d8d6ce;
}

button.selected {
  background: #5c5c5c;
  color: white;
}

button:active {
  transform: scale(0.97);
}

.controls button {
  padding: 0.6rem 0.2rem;
  border-radius: 999px;
  border: 1.5px solid black;
  background: white;
  font-weight: 500;
}

.controls button:disabled {
  opacity: 0.4;
  border-color: #aaa;
  color: #aaa;
}

.controls button:last-child:not(:disabled) {
  background: black;
  color: white;
  border-color: black;
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
.solved-row {
  height: clamp(70px, 10vw, 110px);
  border-radius: 12px;
  margin-bottom: 0.75rem;
  padding: 0.6rem 1rem;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

  animation: fadeIn 0.3s ease;
}

.solved-category {
  font-weight: 800;
  font-size: 0.95rem;
  letter-spacing: 0.04em;
  margin-bottom: 0.25rem;
}

.solved-words {
  font-weight: 400;
  font-size: 0.78rem;
  letter-spacing: 0.015em;
  opacity: 0.85;
}

.category {
  font-size: 0.9rem;
}

.group-0 { background: #f9df6d; }
.group-1 { background: #a0c4ff; }
.group-2 { background: #caffbf; }
.group-3 { background: #ffd6a5; }

.solved-tile {
  aspect-ratio: 1 / 1;
  border-radius: 8px;
  font-weight: 700;
  display: flex;
  align-items: center;
  justify-content: center;
  letter-spacing: 0.045em;
  font-size: clamp(0.62rem, 2.8vw, 0.9rem);
  color: #1c1c1c;
}
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

</style>
