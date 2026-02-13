<script lang="ts">
	import favicon from '$lib/assets/favicon.svg';
	import '../app.css'
	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
	<title>happy valentines day I love you!</title>
</svelte:head>


{@render children()}
