<script>
  
  import Hero from '$lib/sections/Hero.svelte';
  import Essay from '$lib/sections/Essay.svelte';
  import Connections from '$lib/sections/Connections.svelte';
  import Postcard from '$lib/sections/Postcard.svelte';
  import PhotoMulti from '$lib/sections/PhotoMulti.svelte';
  import ParallaxHero from '$lib/sections/ParallaxHero.svelte';
  import SpotifySection from '$lib/sections/SpotifySection.svelte';
  import BookStrip from '$lib/sections/BookStrip.svelte';
  import Ily from '$lib/sections/Ily.svelte';
  import essay01data from '$lib/data/essays/essay-01.json';
  import essay02data from '$lib/data/essays/essay-02.json';
  import essay03data from '$lib/data/essays/essay-03.json';
  import booksData from '$lib/data/books.json';
  import data from '$lib/data/photomulti/set-a.json';
  import data2 from '$lib/data/photomulti/set-b.json'
  import data3 from '$lib/data/photomulti/set-c.json';
  import data4 from '$lib/data/photomulti/set-d.json'
 
  
</script>

<main>
  <Hero />
  <!--1. artifacts-->
  <PhotoMulti {data}/>
  <Essay data={essay01data} />
  <ParallaxHero src="./images/parallax0.jpg" alt="Cinematic landscape"/>
  <Connections />
  <!--2. horses-->
  <PhotoMulti data={data2} />
  <Essay data={essay02data} />
  <PhotoMulti data={data3} />
  <Essay data={essay03data} />
  <ParallaxHero src="./images/parallax3.jpg" alt="Cinematic landscape"/>
  <BookStrip data={booksData} />
  <!--3. horses-->
 
  <SpotifySection playlistId="702SfCpEMJ7ExLFbjgWEFT"/> 
  <!--4. horses-->
  <PhotoMulti data={data4} />
  <Postcard />
  <Ily />
</main>



<style>
  main {
    display: flex;
    flex-direction: column;
    background-color: white;
  }
</style>
