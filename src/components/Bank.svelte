<script>
	export let gameContract;
	$: currentBankData = {};
	const getBank = async () => {
		currentBankData = await gameContract.getBigBoss();
	};
	const battleEar = async () => {
		console.log('battleEar');
		gameContract.on('AttackCompleted', (_) => {
			getBank();
			console.log('AttackCompleted - bank');
		});
	};
	battleEar();
	getBank();
</script>

<div class="bank">
	<img src={currentBankData.imageURI} alt={currentBankData.name} />
	<h2>{currentBankData.name}</h2>
	<div class="stats">
		<div class="hp">Health: {currentBankData.hp} / {currentBankData.maxHp}</div>
		<div class="dmg">Attack: {currentBankData.attackDamage}</div>
	</div>
</div>

<style>
	img {
		display: block;
		margin-left: auto;
		margin-right: auto;
		width: 25%;
	}
	.bank {
		width: 33%;
		height: 33%;
		margin-left: auto;
		margin-right: auto;
	}
	@media screen and (max-width: 600px) {
		.bank {
			width: 100%;
			height: 100%;
		}
	}
	.bank img {
		width: 100%;
		height: 100%;
		object-fit: cover;
		margin-bottom: -20px;
		border-top-left-radius: 10px;
		border-top-right-radius: 10px;
	}
	.bank h2 {
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
