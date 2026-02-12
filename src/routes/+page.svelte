<script>
  
  import Hero from '$lib/sections/Hero.svelte';
  import Essay from '$lib/sections/Essay.svelte';
  import essay01data from '$lib/data/essays/essay-01.json';
  import Connections from '$lib/sections/Connections.svelte';
  import Postcard from '$lib/sections/Postcard.svelte';
  import PhotoMulti from '$lib/sections/PhotoMulti.svelte';
  import ParallaxHero from '$lib/sections/ParallaxHero.svelte';
  import SpotifySection from '$lib/sections/SpotifySection.svelte';
  import BookStrip from '$lib/sections/BookStrip.svelte';
  import booksData from '$lib/data/books.json';
  import data from '$lib/data/photomulti/set-a.json';
  import data2 from '$lib/data/photomulti/set-b.json'
</script>

<main>
  <Hero />
  <PhotoMulti data={data2}/>
  <Essay data={essay01data} />
  <ParallaxHero src="./images/paralax.jpg" alt="Cinematic landscape"/>
  <Connections />
  <PhotoMulti {data} />
  <BookStrip data={booksData} />
  <SpotifySection playlistId="702SfCpEMJ7ExLFbjgWEFT"/> 
  <Postcard />
</main>



<style>
  main {
    display: flex;
    flex-direction: column;
    background-color: white;
  }
</style>
