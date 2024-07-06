<template>
	<button
		@click="ConnectWallet"
		v-if="!wallet"
		class="rounded-md text-lg font-medium transition-colors text-white h-10 px-2 py-2 mb-5 w-2/4"
		style="background-color: #ff6666"
	>
		Connect UniSat
	</button>
	<button
		@click.prevent.stop="$emit('func')"
		v-else
		class="rounded-md text-lg font-medium transition-colors text-white h-10 px-2 py-2 mb-5 w-2/4"
		style="background-color: #ff6666"
	>
		<slot />
	</button>
</template>

<script setup>
import { useWallet } from '../composables/useState';

const { func } = defineProps(['func']);

const wallet = useWallet();

async function ConnectWallet() {
	if (typeof window.unisat == 'undefined') {
		alert('You need to install UniSat');
	}

	try {
		let accounts = await window.unisat.requestAccounts();
		wallet.value = window.unisat;
		console.log('connect success', accounts);
	} catch (e) {
		return console.log('connect failed');
	}
}
</script>
