<script>
	export let gameContract;
	$: heros = [];
	const getHeros = async () => {
		heros = await gameContract.getAllHeros();
		console.log(heros);
	};
	const battleEar = async () => {
		console.log('battleEar');
		gameContract.on('AttackCompleted', (_) => {
			getHeros();
			console.log('AttackCompleted - hero');
		});
	};
	const mintEar = async () => {
		console.log('mintEar');
		gameContract.on('CharacterNFTMinted', (_) => {
			getHeros();
			console.log('A new hero joins the fray!');
		});
	};
	getHeros();
	battleEar();
	mintEar();
</script>

<h2>Your Heros</h2>
<div class="heros">
	{#each heros as hero}
		<div class="hero">
			<img src={hero.imageURI} alt="{hero.name}}" />
			<h2>{hero.name}</h2>
			<div class="stats">
				<div class="hp">Health: {hero.hp} / {hero.maxHp}</div>
				<div class="dmg">Attack: {hero.attackDamage}</div>
			</div>
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
			/* create grid with 3 columns */
			/* Have images fill grid */
			/* Have images be centered */
			display: grid;
			grid-template-columns: 1fr 1fr 1fr;
			grid-gap: 10px;
			/* On small screens make single column */
		}
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
	h2 {
		text-align: center;
	}
	.stats {
		display: flex;
		flex-direction: row;
		justify-content: space-around;
		align-items: center;
		margin-top: 10px;
	}
</style>
