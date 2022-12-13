<svelte:options tag="fl-catbus" />

<script>
	import { count } from "./lib/stores.js";
	import { onMount, onDestroy } from "svelte";

	let totoroClicks = 0;

	// Use this to listen to events triggered by other micro-frontends:
	const listener = ({ detail }) => {
		console.log(detail.message);
		totoroClicks++;
	};

	onMount(async () => {
		window.addEventListener("TOTORO_WAS_CLICKED", listener);
	});

	onDestroy(async () => {
		window.removeEventListener("TOTORO_WAS_CLICKED", listener);
	});
</script>

<main part="mfeMain">
	<h1 part="mfeMainH1">MFE 2: Catbus (ネコバス, Nekobasu)</h1>
	<img
		part="mfeMainImg"
		src="/img/catbus.jpeg"
		alt="Catbus is a large character who is both a cat, and a bus."
	/>
	<p>
		I am Catbus. I listen to the <code>TOTORO_WAS_CLICKED</code> event,
		which is triggered by my MFE neighbor Totoro's first child component. So
		far, Totoro's first child was clicked {totoroClicks} time(s).
	</p>
	<p>
		Stores can be used with MFEs to keep the app state of each MFE, but they
		cannot be used to share a global state across MFEs. I you click on the
		first MFE, this count here will still show zero because the Count on the
		first MFE cannot affect the Count on the second MFE.
	</p>
	<p>
		This is good for isolation. This also means that stores cannot be used
		for MFE communication - we use events instead.
	</p>
	<p>Clicks: {$count}.</p>
</main>

<style>
	p {
		font-size: small;
	}
</style>
