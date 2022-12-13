<svelte:options tag="fl-catbus" />

<script>
	import { count } from "./lib/stores.js";
</script>

<main part="mfeMain">
	<h1 part="mfeMainH1">MFE 2: Catbus (ネコバス, Nekobasu)</h1>
	<img
		part="mfeMainImg"
		src="/img/catbus.png"
		alt="Catbus is a large character who is both a cat, and a bus."
	/>
	<p>
		Stores can be used with MFEs to keep the app state of each MFE, but they
		cannot be used to share a global state across MFEs. I you click on
		the first MFE, this count here will still show zero because the Count on
		the first MFE cannot affect the Count on the second MFE.
	</p>
	<p>
		This is good for isolation. This also means that stores cannot be
		used for MFE communication -  we use a message bus instead.
	</p>
	<p>Clicks: {$count}.</p>
</main>

<style>
	p {
		font-size: small;
	}
</style>
