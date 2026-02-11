<script lang="ts">
  let imageFile: File | null = null;
  let imageUrl: string | null = null;
  let message = '';
  let address = '';


  function handleImageUpload(e: Event) {
    const input = e.target as HTMLInputElement;
    if (!input.files?.length) return;

    imageFile = input.files[0];
    imageUrl = URL.createObjectURL(imageFile);
  }

function wrapText(
  ctx: CanvasRenderingContext2D,
  text: string,
  x: number,
  y: number,
  maxWidth: number,
  lineHeight: number
) {
  const paragraphs = text.split('\n');
  let offsetY = 0;

  for (const para of paragraphs) {
    const words = para.split(' ');
    let line = '';

    for (let i = 0; i < words.length; i++) {
      const testLine = line + words[i] + ' ';
      const metrics = ctx.measureText(testLine);

      if (metrics.width > maxWidth && i > 0) {
        ctx.fillText(line, x, y + offsetY);
        line = words[i] + ' ';
        offsetY += lineHeight;
      } else {
        line = testLine;
      }
    }

    ctx.fillText(line, x, y + offsetY);
    offsetY += lineHeight * 1.2;
  }
}


async function downloadPostcard() {
  await (document as any).fonts.load('28px "FF Magda Pro Cameo"');

  if (!imageFile) return;

  const uploaded = new Image();
  uploaded.src = imageUrl!;
  await uploaded.decode();

  const template = new Image();
  template.src = '/images/postcard.jpg';
  await template.decode();

  const border = 14;

  const targetWidth = 1200;
  const imgRatio = uploaded.width / uploaded.height;

  // image defines size directly
  let drawW = targetWidth;
  let drawH = drawW / imgRatio;


  /* --------------------------------------------------
     STEP 2: Define postcard size FROM image + border
  -------------------------------------------------- */

  const cardWidth = drawW + border * 2;
  // FRONT HEIGHT = image + border
  const frontHeight = drawH + border * 2;

  // BACK HEIGHT = template scaled to match width
  const backHeight = Math.round(
    cardWidth * (template.height / template.width)
  );


  /* --------------------------------------------------
     STEP 3: Create canvas
  -------------------------------------------------- */

  const canvas = document.createElement('canvas');
  canvas.width = cardWidth;
  canvas.height = frontHeight + backHeight;


  const ctx = canvas.getContext('2d')!;

  /* --------------------------------------------------
     STEP 4: FRONT PANEL BACKGROUND ONLY
     (Prevents beige border around template)
  -------------------------------------------------- */

  ctx.fillStyle = '#f7f3ea';
  ctx.fillRect(0, 0, cardWidth, frontHeight);

  /* --------------------------------------------------
     STEP 5: Draw FRONT (uploaded image)
  -------------------------------------------------- */

  const x = border;

  // 🔧 FIX: anchor image to top instead of vertical centering
  const y = border;

  ctx.fillStyle = '#fff';
  ctx.fillRect(
    x - border,
    y - border,
    drawW + border * 2,
    drawH + border * 2
  );

  ctx.drawImage(uploaded, x, y, drawW, drawH);

  /* --------------------------------------------------
     STEP 6: Draw BACK (template exactly once)
     No background fill here → no beige border
  -------------------------------------------------- */

  ctx.drawImage(template, 0, frontHeight, cardWidth, backHeight);


  /* --------------------------------------------------
     STEP 7: Postcard Text
  -------------------------------------------------- */

  const backTop = frontHeight;

  ctx.fillStyle = '#1f3a6f';
  ctx.textAlign = 'left';

  // MESSAGE (LEFT)
  ctx.font = '28px "FF Magda Pro Cameo"';

  const messageX = 120;
  const messageY = backTop + 180;
  const messageWidth = cardWidth / 2 - 160;

  wrapText(
    ctx,
    message || 'Wish you were here.',
    messageX,
    messageY,
    messageWidth,
    40
  );

  // ADDRESS (RIGHT)
  ctx.font = '26px "FF Magda Pro Cameo"';

  const addressX = cardWidth / 2 + 70;
  const addressY = backTop + 220;
  const addressWidth = cardWidth / 2 - 160;

  wrapText(
    ctx,
    address || 'Recipient Name\nStreet Address\nCity, Country',
    addressX,
    addressY,
    addressWidth,
    38
  );

  /* --------------------------------------------------
     STEP 8: Download
  -------------------------------------------------- */

  const link = document.createElement('a');
  link.download = 'postcard.png';
  link.href = canvas.toDataURL('image/png');
  link.click();
}




</script>
<section class="postcard snap">
  <div class="layout">
    <div class="controls">
      <h2>send me postcards :)</h2>

      <label>
        Image
        <input type="file" accept="image/*" on:change={handleImageUpload} />
      </label>

      <label>
        Message
        <textarea bind:value={message} rows="4" />
      </label>

      <label>
        Address
        <textarea bind:value={address} rows="4" />
      </label>


      <button on:click={downloadPostcard}>
        Download Postcard
      </button>
    </div>

    <div class="preview">
      <div class="mock">
        {#if imageUrl}
          <img src={imageUrl} alt="Preview" />
        {/if}
        <p>{message || 'Your message will appear here…'}</p>
      </div>
    </div>
  </div>
</section>
<style>
  section {
    min-height: 100vh;
    padding: clamp(3rem, 8vw, 6rem) 1.5rem;
    background: #f7f3ea;
  }

  .layout {
    max-width: 1200px;
    margin: auto;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 3rem;
  }

  .controls label {
    display: block;
    margin-bottom: 1rem;
  }

  textarea,
  input {
    width: 100%;
    margin-top: 0.4rem;
  }

  button {
    margin-top: 1.5rem;
    padding: 0.75rem 1.5rem;
  }

  .preview {
    display: grid;
    place-items: center;
  }

  .mock {
    width: 320px;
    aspect-ratio: 3 / 2;
    background: white;
    padding: 1rem;
    box-shadow: 0 20px 40px rgba(0,0,0,0.15);
    display: grid;
    grid-template-rows: 1fr auto;
    gap: 0.5rem;
  }

  .mock img {
    width: 100%;
    object-fit: cover;
  }

  .mock p {
    font-size: 0.85rem;
    text-align: center;
  }

  @media (max-width: 800px) {
    .layout {
      grid-template-columns: 1fr;
    }
  }
</style>
