<script>
	import { browser } from '$app/env';
	import bear from "../lib/bearuwu.svg";
	import { defaultEvmStores, web3, selectedAccount, connected, chainId, chainData } from 'svelte-web3';
	import {page} from "$app/stores";

	if (browser) {
		defaultEvmStores.setBrowserProvider();
	}

	let address;
	let currentBalance;
	const query_balance = async () => {
		if($web3.utils.isAddress(address)) currentBalance = $web3.utils.fromWei(await $web3.eth.getBalance(address)).toString() + " " + $chainData?.nativeCurrency?.symbol;
		else currentBalance = "Not a valid address.";
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
	<form on:submit|preventDefault={query_balance} class="m-2 p-2">
		<input bind:value={address} class="h-9 wallet-address-box p-3" placeholder="Input Wallet Address"/>
		<button type="submit">Get Balance</button>
	</form>
		<div bind:textContent={currentBalance} contenteditable="false"></div>
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

	.wallet-address-box {
		border-radius: 10px;
	}
</style>
