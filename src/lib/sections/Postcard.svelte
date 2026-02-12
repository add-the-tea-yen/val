<script lang="ts">
  let uploadedImage: string | null = null;

  function handleImageUpload(e: Event) {
    const file = (e.target as HTMLInputElement).files?.[0];
    if (!file) return;

    const reader = new FileReader();
    reader.onload = () => {
      uploadedImage = reader.result as string;
    };
    reader.readAsDataURL(file);
  }

  let message = '';
  let recipient = '';

</script>

<section class="postcard-section">

<div class="postcard-inner">
  <div class="postcard-header">
<h2>send me a postcards :)</h2>

<div class="upload-area">
  <label class="download">
    add an image
    <input type="file" accept="image/*" on:change={handleImageUpload} hidden />
  </label>
</div>

</div>
  
<div class="postcard-frame">
  <div class="photo-preview">
  {#if uploadedImage}
    <img src={uploadedImage} alt="Uploaded preview" />
  {/if}
</div>


  <!-- Postcard Preview -->
  <div class="card postcard-template">

  <img src="./images/postcard.jpg" alt="Postcard Template"/>

  <!-- MESSAGE INPUT -->
  <textarea
    bind:value={message}
    class="message-input"
    placeholder="message"
  ></textarea>

  <!-- NAME INPUT -->
  <textarea
    bind:value={recipient}
    class="recipient-input"
    placeholder="address"
  ></textarea>

</div>

    <!-- LIVE NAME -->
    <div class="recipient">
      {recipient || ''}
    </div>
</div>

</div>

</section>


<style>
.postcard-section {
  min-height: 100svh;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
  margin: 0;
  justify-content: center;   /* vertical center */
  align-items: center;
}

.postcard-section h2 {
  margin-bottom: 0.5rem;
}

.postcard-inner {
  width: min(700px, 95vw);
  
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

h2 {
  font-family: 'Helvetica Neue',sans-serif;
  font-size: clamp(1.2rem, 4vw, 2.2rem);
  font-weight: 600;
  color: #1f3a6f;
}

/* ====================
   COMPOSER
==================== */



textarea {
  width: 100%;
  height: 220px;
  border: none;
  background: black;
  color: #ccc;
  font-size: 1.4rem;
  padding: 1rem;
  resize: none;
}



/* ====================
   DOWNLOAD BUTTON
==================== */

.download {
  background: white;
  color: #1f3a6f;
  font-family: 'Helvetica Neue',sans-serif;
  font-weight: 400;
  font-size: clamp(0.8rem, 4vw, 1.4rem);
  cursor: pointer;
}

.download:hover {
  text-decoration: underline;
}


.card {
  position: relative;
  width: 100%;
}


.card img {
  width: 100%;
  display: block;
}

/* MESSAGE AREA */


/* ADDRESS AREA */
.recipient {
  position: absolute;
  top: 38%;
  right: 8%;
  width: 32%;

  font-size: clamp(0.8rem, 1.3vw, 1rem);
  color: #1f3a6f;
}
.card {
  position: relative;
  width: min(700px, 95vw);
}

.card img {
  width: 100%;
  display: block;
}

/* MESSAGE AREA */
.message-input {

  position: absolute;
  top: 10%;
  left: 6%;
  width: 40%;
  height: 85%;

  background: transparent;
  border: none;
  resize: none;

  font-family: 'Inter',sans-serif;
  font-weight: 500;
  font-size: clamp(0.8rem, 1.4vw, 1.1rem);
  line-height: 1.5;
  color: #1f3a6f;

  outline: none;
  -ms-overflow-style: none;
    scrollbar-width: none;
}

/* ADDRESS AREA */
.recipient-input {

  position: absolute;
  top: 41%;
  right: 12%;
  width: 32%;
  height: 35%;

  background: transparent;
  border: none;
  resize: none;

  font-family: 'Inter',sans-serif;
  font-weight: 600;
  font-size: clamp(1.2rem, 2.2vw, 1rem);
  color: #1f3a6f;

  outline: none;

  -ms-overflow-style: none;
    scrollbar-width: none;
}

.recipient-input::-webkit-scrollbar{
  display: none;
}
.message-input::-webkit-scrollbar{
  display: none;
}

/* Upload box */

.upload-area {
  display: flex;
  justify-content: center;
  margin-bottom: 1rem;
}



/* Uploaded image */

.photo-preview {
  width: 100%;
  aspect-ratio: 3 / 2;
  background: #f3eed8;
  position: relative;
  overflow: hidden;
}

.photo-preview img {
  max-width: 96%;
  max-height: 96%;

  width: auto;
  height: auto;

  object-fit: contain;

  position: absolute;
  inset: 0;
  margin: auto;
}

.postcard-frame {
  width: min(700px, 95vw);
  display: flex;
  flex-direction: column;
  gap: 1rem;
}


.postcard-template {
  position: relative;
  width: 100%;
}

.postcard-header {
  width: min(700px, 95vw);
  align-self: stretch;
  margin: 0 auto;
  text-align: left;
}

.postcard-header h2 {
  margin-bottom: 0.4rem;
}

.upload-area {
  justify-content: flex-start;
}


</style>
