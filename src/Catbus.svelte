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

<div part="mfeCard">
	<div part="mfeCardTitle">Micro-Frontend</div>
	<div part="mfeCardBody">
		<main part="mfeMain">
			<h1 part="mfeMainH1">MFE 2: Catbus (ネコバス, Nekobasu)</h1>
			<div class="flex">
				<img
					part="mfeMainImg"
					src="/img/catbus.jpeg"
					alt="Catbus is a large character who is both a cat, and a bus."
				/>
				<div part="mfeCard">
					<div part="mfeCardTitle">Stores owned by Catbus</div>
					<div part="mfeCardBody" class="mfeCardBody">
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
	p {
		font-size: small;
	}
	.flex {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		margin-bottom: 25px;
	}
	.mfeCardBody {
		padding: 10px;
	}
</style>
