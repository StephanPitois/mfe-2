<svelte:options tag="fl-catbus" />

<script>
	import { count } from "./lib/stores.js";
	import { registerEventListeners } from "./lib/EventListeners";

	let totoroClicks = 0;
	let totoroEvents = [];

	// Use this to listen to events triggered by other micro-frontends:
	const listener = ({ detail }) => {
		console.log(detail.message);
		totoroClicks++;
		console.log("Catbus heard TOTORO_WAS_CLICKED.");
		totoroEvents = [
			...totoroEvents,
			{
				ts: new Date(),
				message: "TOTORO_WAS_CLICKED",
			},
		];
	};

	registerEventListeners("MFE-2: Catbus", [
		{
			eventType: "TOTORO_WAS_CLICKED",
			listener,
		},
	]);
</script>

<div class="mfeCard wrapper">
	<div class="mfeCardTitle">Micro-Frontend</div>
	<div class="mfeCardBody">
		<main class="mfeMain">
			<h1 class="mfeMainH1">MFE 2: Catbus (ネコバス, Nekobasu)</h1>
			<div class="flex">
				<img
				class="mfeMainImg"
					src="/img/catbus.jpeg"
					alt="Catbus is a large character who is both a cat, and a bus."
				/>
				<div class="mfeCard">
					<div class="mfeCardTitle">Stores owned by Catbus</div>
					<div class="mfeCardBody">
						<code>$count = {$count}</code>
					</div>
				</div>
			</div>
			<p>
				I am Catbus. I listen to the <code>TOTORO_WAS_CLICKED</code>
				event, which is triggered by my MFE neighbor Totoro's first child
				component Satsuki. So far, Totoro's first child Satsuki triggered
				{totoroClicks}
				event(s). NOTE: my memory is not very good and unlike the app shell,
				I don't remember previous events when you navigate back and forth
				between Page 1 and Page 2. When you come back from Page 2, you will
				see that my event count is back to zero. I could save that state
				in my store but it's not very important for this demo.
				<br /><br />
				{#if totoroEvents.length}
					{#each totoroEvents as totoroEvent}
						<code>
							{new Date(totoroEvent.ts).toLocaleTimeString()}: {totoroEvent.message}
						</code>
						<br />
					{/each}
				{:else}
					<i>Waiting for TOTORO_WAS_CLICKED...</i>
				{/if}
			</p>
			<p>
				The current value of Catbus's store $count is {$count}. No code
				is written to change that value. This store is different from
				the shell store of the same name, and from Totoro's store of the
				same name.
			</p>
		</main>
	</div>
</div>

<style>
	/* 
	@import url seems to be an acceptable way to share global styles.
	@import seems to only import the style once, even when called
	by several components on the same page. And from what I've seen
	so far, the imported styles don't seem to be inlined in the 
	generated web component, which is good as that prevents
	duplication and bloat. global.css is defined in the app shell.
	*/
	@import "/global.css";
	p {
		font-size: small;
	}
	.flex {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		margin-bottom: 25px;
	}
	.wrapper .mfeCard .mfeCardBody {
		padding: 10px;
	}
</style>
