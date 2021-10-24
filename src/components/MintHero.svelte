<script>
	export let gameContract;
	$: heros = [];
	$: minting = false;
	const getHeros = async () => {
		heros = await gameContract.getAllDefaultCharacters();
	};
	const mintHero = async (characterIndex) => {
		const txn = await gameContract.mintCharacterNFT(characterIndex);
		console.log('minting');
		minting = true;
		await txn.wait();
		minting = false;
	};
	getHeros();
</script>

<h2>Mint a Hero!</h2>
<h3>It's dangerous to go alone</h3>
{#if minting}
	<div id="modal">
		<p>🏭 MINTING! 🏭</p>
		<img src="/mint.gif" alt="minting" />
	</div>
{/if}
<div class="heros">
	{#each heros as hero}
		<div class="hero">
			<img src={hero.imageURI} alt={hero.name} on:click={mintHero(hero.characterIndex)} />
			<h2>{hero.name}</h2>
		</div>
	{/each}
</div>

<style>
	.heros {
		/* create grid with 3 columns */
		/* Have images fill grid */
		/* Have images be centered */
		display: grid;
		grid-template-columns: 1fr 1fr 1fr;
		grid-gap: 10px;
		/* On small screens make single column */
	}
	@media screen and (max-width: 600px) {
		.heros {
			grid-template-columns: 1fr;
			/* space between grid row */
			grid-row-gap: 3rem;
		}
	}
	.hero {
		width: 100%;
		height: 100%;
	}
	.hero img {
		width: 100%;
		height: 100%;
		object-fit: cover;
		margin-bottom: -20px;
		border-top-left-radius: 10px;
		border-top-right-radius: 10px;
	}
	.hero h2 {
		background-color: darkorchid;
		color: white;
		/* round bottom corners */
		border-bottom-left-radius: 10px;
		border-bottom-right-radius: 10px;
		/* cover more of image */
		/* padding: 10px; */
	}
	/* show text on hover */
	.hero:hover {
		opacity: 0.5;
	}
	h2,
	h3 {
		text-align: center;
	}
	img {
		display: block;
		margin-left: auto;
		margin-right: auto;
		width: 100%;
	}
	#modal {
		position: fixed;
		z-index: 2;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		background: #fff;
		filter: drop-shadow(0 0 20px #333);
	}
	#modal p {
		font-size: 2rem;
		text-align: center;
	}
</style>
