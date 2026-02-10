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
  await (document as any).fonts.load('28px "Magda Cameo Regular"');

  if (!imageFile) return;

  const uploaded = new Image();
  uploaded.src = imageUrl!;
  await uploaded.decode();

  const template = new Image();
  template.src = '/images/postcard.jpg';
  await template.decode();

  const cardWidth = 1400;
  const cardHeight = Math.round(
    cardWidth * (template.height / template.width)
  );

  // canvas
  const canvas = document.createElement('canvas');
  canvas.width = cardWidth;
  canvas.height = cardHeight * 2; // front + back

  const ctx = canvas.getContext('2d')!;

  // FILL ENTIRE CANVAS ONCE (important)
  ctx.fillStyle = '#f7f3ea';
  ctx.fillRect(0, 0, canvas.width, canvas.height);

  const padding = 40;
  const border = 14;

  // available area for photo (FRONT PANEL)
  const areaW = cardWidth - padding * 2;
  const areaH = cardHeight - padding * 2;

  // image cover math
  const imgRatio = uploaded.width / uploaded.height;
  const areaRatio = areaW / areaH;

  let drawW, drawH;

  if (imgRatio > areaRatio) {
    drawH = areaH;
    drawW = drawH * imgRatio;
  } else {
    drawW = areaW;
    drawH = drawW / imgRatio;
  }

  const x = (cardWidth - drawW) / 2;

  // 🔧 FIX: y is relative to FRONT PANEL (0 → cardHeight)
  const y = (cardHeight - drawH) / 2;

  // white border
  ctx.fillStyle = '#fff';
  ctx.fillRect(
    x - border,
    y - border,
    drawW + border * 2,
    drawH + border * 2
  );

  // draw uploaded image (FRONT)
  ctx.drawImage(uploaded, x, y, drawW, drawH);

  // ✅ DRAW POSTCARD BACK EXACTLY ONCE
  ctx.drawImage(template, 0, cardHeight, cardWidth, cardHeight);

  /* POSTCARD TEXT — BACK PANEL ONLY */

  /* POSTCARD TEXT — BACK PANEL ONLY */

  const backTop = cardHeight;

  // ink color
  ctx.fillStyle = '#1f3a6f';
  ctx.textAlign = 'left';

  // MESSAGE (LEFT SIDE)
  ctx.font = '28px "FF Magda Pro Cameo"';

  const messageX = 90;
  const messageY = backTop + 160;   // ↓ pushed below printed header
  const messageWidth = cardWidth / 2 - 160;

  wrapText(
    ctx,
    message || 'Wish you were here.',
    messageX,
    messageY,
    messageWidth,
    40
  );

  // ADDRESS (RIGHT SIDE)
  ctx.font = '26px "FF Magda Pro Cameo"';

  const addressX = cardWidth / 2 + 70;
  const addressY = backTop + 200;   // ↓ aligned with real address zone
  const addressWidth = cardWidth / 2 - 160;

  wrapText(
    ctx,
    address || 'Recipient Name\nStreet Address\nCity, Country',
    addressX,
    addressY,
    addressWidth,
    38
  );


  /* DOWNLOAD */
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
        Message (left side)
        <textarea bind:value={message} rows="4" />
      </label>

      <label>
        Address (right side)
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
