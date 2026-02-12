<section class="hero">
  <div class="frame">
    <img src="./images/cover.jpg" alt="cover"/>
  </div>
</section>

<style>
.hero {
  min-height: 100vh;
  background: #ffd400;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: clamp(2rem, 6vw, 5rem);
  box-sizing: border-box;
}


  /* IMAGE FRAME */
.frame {
  height: min(85vh, 900px);
  aspect-ratio: 3 / 4.5;
  display: flex;
  align-items: center;
  justify-content: center;
}

.frame img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  display: block;
}

@media (max-width: 600px) {
  .frame {
    height: min(75vh, 600px);
  }
}

</style>
