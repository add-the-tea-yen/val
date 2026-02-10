<script>
  import Hero from '$lib/sections/Hero.svelte';
  import Essay from '$lib/sections/Essay.svelte';
  import Connections from '$lib/sections/Connections.svelte';
  import Postcard from '$lib/sections/Postcard.svelte';
  import PhotoMulti from '$lib/sections/PhotoMulti.svelte';
  import data from '$lib/data/photomulti/set-a.json';
</script>

<main>

  <Essay />
  <Connections />
  <PhotoMulti {data} />

</main>

<style>
  main {
    display: flex;
    flex-direction: column;
    background-color: white;
  }
</style>
