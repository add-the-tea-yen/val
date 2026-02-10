<script>
  
  import Hero from '$lib/sections/Hero.svelte';
  import Essay from '$lib/sections/Essay.svelte';
  import essay01data from '$lib/data/essays/essay-01.json';
  import Connections from '$lib/sections/Connections.svelte';
  import Postcard from '$lib/sections/Postcard.svelte';
  import PhotoMulti from '$lib/sections/PhotoMulti.svelte';
  import data from '$lib/data/photomulti/set-a.json';
</script>

<main>
  <Hero />
  <Essay data={essay01data} />
  <Connections />
  <PhotoMulti {data} />
  <Postcard />
</main>



<style>
  main {
    display: flex;
    flex-direction: column;
    background-color: white;
  }
</style>
