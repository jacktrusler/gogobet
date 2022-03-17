<script>
	import { 
		defaultEvmStores, 
		web3, 
		selectedAccount, 
		connected, 
		chainId, 
		chainData } from 'svelte-web3';
	import { browser } from '$app/env';
	import bear from "../lib/bearuwu.svg";

	if (browser) {
		defaultEvmStores.setBrowserProvider();
	}

	let address;
	let currentBalance;
	const query_balance = async () => {
		if($web3.utils.isAddress(address)) {
			currentBalance = $web3.utils
			.fromWei(await $web3.eth.getBalance(address))
			.toString() + " " + $chainData?.nativeCurrency?.symbol;
		}
			
		else currentBalance = "Not a valid address.";
	}

	let walletId = '';
	const query_address = async () => {
  	const accounts = await $web3.eth.getAccounts();
  	
		walletId = accounts;
	}

	let username = ''; 
	let password = '';
	let passwordVerify = ''; 	

	const validate = () => {
		if (password === passwordVerify){	
			query_address()	
		} else {
			console.log('passwords dont match king');
		}
	}

	const logger = () => {
		console.log(username);
		console.log(password);
		console.log(walletId);
	}

</script>

<script context="module" lang="ts">
	export const prerender = true;
</script>

<section>
	<div class="flex flex-col justify-center items-center">
		<h1 class="font-bold text-yellow-600">GoGoBet!</h1>
		<a href="/bet">
			<img src={bear} alt="bearuwu" />
			</a>
			<h1 class="font-bold text-yellow-600">Click on GoGoBear to bet!</h1>
			<p class="font-bold text-xl">change currency with metamask addon</p>
		<p class="font-bold text-xl">{$chainData?.name} </p>
		<p>Native Currency: {$chainData?.nativeCurrency?.name} ({$chainData?.nativeCurrency?.symbol})</p>
	</div>

		<form on:submit|preventDefault={validate}>
			<div class='p-3'></div>
			<input bind:value={username} class="h-9 wallet-address-box p-3" placeholder="Username">
			<div class='p-3'></div>
			<input bind:value={password} class="h-9 wallet-address-box p-3" placeholder="Password">
			<div class='p-3'></div>
			<input bind:value={passwordVerify} class="h-9 wallet-address-box p-3" placeholder="Re-enter Password">
			<div class='p-3'></div>
			<button type="submit" on:click={logger}>Submit</button>
		</form>
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 1;
	}

	h1 {
		width: 100%;
	}

	.address-button{
		border: 2px solid black;
	}

	.wallet-address-box {
		border-radius: 10px;
	}
</style>
