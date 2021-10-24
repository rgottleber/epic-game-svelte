<script>
	export let gameContract;
	$: currentHeroData = {};
	const getHero = async () => {
		currentHeroData = await gameContract.checkIfUserHasNFT();
	};
	const battleEar = async () => {
		console.log('battleEar');
		gameContract.on('AttackCompleted', (_) => {
			getHero();
			console.log('AttackCompleted - hero');
		});
	};
	getHero();
	battleEar();
</script>

<h2>Your Hero</h2>
<div class="hero">
	<img src={currentHeroData.imageURI} alt={currentHeroData.name} />
	<h2>{currentHeroData.name}</h2>
	<div class="stats">
		<div class="hp">Health: {currentHeroData.hp} / {currentHeroData.maxHp}</div>
		<div class="dmg">Attack: {currentHeroData.attackDamage}</div>
	</div>
</div>

<style>
	img {
		display: block;
		margin-left: auto;
		margin-right: auto;
		width: 25%;
	}
	.hero {
		width: 20%;
		height: 20%;
		margin-left: auto;
		margin-right: auto;
	}
	@media screen and (max-width: 600px) {
		.hero {
			width: 100%;
			height: 100%;
		}
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
		/* center text */
		text-align: center;
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
