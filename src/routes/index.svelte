<script>
	import { onMount } from 'svelte';
	import { ethers } from 'ethers';
	import MyEpicGame from '../contracts/MyEpicGame.json';
	import Arena from '../components/Arena.svelte';
	$: account = null;
	$: chainID = null;
	let web3Props;
	const contractAddress = '0x52F8B8508A8bf8e793C9EC94f654C9f79d73F6e3';
	onMount(async () => {
		try {
			// Check if Web3 has been injected by the browser (MetaMask)
			const { ethereum } = window;
			// If not, prompt for MetaMask
			if (!ethereum) {
				alert('Get MetaMask!');
				return;
			}
			// Get the provider
			const provider = new ethers.providers.Web3Provider(ethereum);
			// Get the signer
			const signer = provider.getSigner();
			// Get the contract
			const gameContract = new ethers.Contract(contractAddress, MyEpicGame.abi, signer);
			// update the props for the components
			web3Props = {
				gameContract
			};
			// Get the accounts
			const accounts = await ethereum.request({ method: 'eth_accounts' });
			// Set the account and chainID
			if (accounts.length !== 0) {
				account = accounts[0];
				chainID = await signer.getChainId();
			}
		} catch (error) {
			console.log(error);
		}
	});
	// Attach Wallet if not already attached
	async function attachWallet() {
		//Get the provider, this time without ethereum object
		const provider = new ethers.providers.Web3Provider(window.ethereum, 'any');
		// Prompt user for account connections
		await provider.send('eth_requestAccounts', []);
		// Get the signer
		const signer = provider.getSigner();
		// Get the account
		account = await signer.getAddress();
		// Get the chainID
		chainID = await signer.getChainId();
	}
</script>

<h1>🏦 The Evil Bank Rizes 🏦</h1>
<p>
	Contract can be found <a href={`https://rinkeby.etherscan.io/address/${contractAddress}`}
		>on Etherscan</a
	>
</p>

{#if !account}
	<p>This application is build on the Rinkeby Test Network.</p>
	<p>Please connect your wallet to Rinkeby Test Network.</p>
	<p>
		<button on:click={attachWallet}>Attach Wallet</button>
	</p>
{:else if chainID != 4}
	<h3>Please use the Rinkeby TestNet</h3>
	<p>Network Name: Rinkeby Testnet</p>
	<p>New RPC URL: https://rinkeby.infura.io/</p>
	<p>Chain ID: 4</p>
	<p>Currency Symbol: ETH</p>
	<p>Block Explorer URL: https://rinkeby.etherscan.io</p>
{:else}
	<Arena {...web3Props} />
{/if}

<style>
	h1,
	h3,
	p {
		text-align: center;
	}
</style>
