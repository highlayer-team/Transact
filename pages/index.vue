<template>
	<div class="flex flex-col items-center justify-center min-h-screen bg-[#131722]">
		<h1 class="text-8xl font-bold text-[#FF6666]">Transact</h1>
		<p class="text-xl text-gray-400">A Simple wallet on Highlayer</p>
		<div class="mt-8 w-[800px] bg-gray-800 rounded-xl shadow-md flex flex-col items-center">
			<div class="flex w-3/4 mt-5 mb-5">
				<button
					class="text-white py-2 px-4 rounded-l-full flex-grow font-bold bg-gray-900 text-lg"
					:class="{
						option: currentPage === 0,
					}"
					@click="currentPage = 0"
				>
					Send
				</button>

				<button
					class="text-white py-2 px-4 flex-grow font-bold bg-gray-900 text-lg"
					:class="{
						option: currentPage === 1,
					}"
					@click="currentPage = 1"
				>
					Transactions
				</button>

				<button
					class="text-white py-2 px-4 rounded-r-full flex-grow font-bold bg-gray-900 text-lg"
					:class="{
						'option ': currentPage === 2,
					}"
					@click="currentPage = 2"
				>
					Faucet
				</button>
			</div>

			<template v-if="currentPage == 0">
				<Transfer />
			</template>
			<!-- <template v-else-if="currentPage == 1">
				<DeployContract />
			</template>
			<template v-else-if="currentPage == 2">
				<TestContract />
			</template> -->
		</div>
	</div>
</template>

<script setup>
import { ref } from 'vue';
import { useWallet } from '../composables/useState';
import Button from '../components/Button.vue';

const wallet = useWallet();

const currentPage = ref(0);
</script>
