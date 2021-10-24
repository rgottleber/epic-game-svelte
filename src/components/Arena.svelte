<script>
	export let gameContract;
	import MintHero from './MintHero.svelte';
	import Bank from './Bank.svelte';
	import Hero from './Hero.svelte';
	import HeroTeam from './HeroTeam.svelte';
	$: currentHeroData = {};
	$: hasHero = false;
	$: battle = false;
	$: heroHP = 0;
	$: bossHP = 0;
	$: attacking = false;
	const getHero = async () => {
		currentHeroData = await gameContract.checkIfUserHasNFT();
		hasHero = currentHeroData.name ? true : false;
		heroHP = currentHeroData.hp;

		// Update the page on event emit
	};
	const mintEar = async () => {
		gameContract.on('CharacterNFTMinted', (_) => {
			getHero();
			console.log('Minted!');
		});
	};
	const attack = async () => {
		const txn = await gameContract.attackBoss();
		attacking = true;
		await txn.wait();
		attacking = false;
	};
	mintEar();
	getHero();
</script>

{#if attacking}
	<div id="modal">
		<p>⚔️ ATTACKING ⚔️</p>
		<img src="/battle.gif" alt="battle" />
	</div>
{/if}
{#if hasHero}
	{#if battle}
		<Bank {gameContract} />
		<p>
			{#if heroHP > 0}
				<button class="attack" on:click={attack}>ATTACK!</button>
			{/if}
			<button class="retreat" on:click={() => (battle = !battle)}>RETREAT!</button>
		</p>
		<HeroTeam {gameContract} />
	{:else}
		<p><button class="attack" on:click={() => (battle = !battle)}>To Battle!</button></p>
		<Hero {gameContract} />
	{/if}
{:else}
	<MintHero {gameContract} />
{/if}

<style>
	.attack {
		/* red gradiant rounded button */
		background: linear-gradient(to right, #ff0000, #ff00ff);
		border-radius: 5px;
		border: none;
		color: white;
		padding: 10px 20px;
		text-align: center;
		text-decoration: none;
		display: inline-block;
		font-size: 16px;
		margin: 4px 2px;
		cursor: pointer;
		/* center left right */
	}
	.retreat {
		/* blue gradiant rounded button */
		background: linear-gradient(to right, #0000ff, #00ffff);
		border-radius: 5px;
		border: none;
		color: white;
		padding: 10px 20px;
		text-align: center;
		text-decoration: none;
		display: inline-block;
		font-size: 16px;
		margin: 4px 2px;
		cursor: pointer;
	}
	p {
		text-align: center;
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
</style>
