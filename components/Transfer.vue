<template>
  <div class="flex flex-col items-center w-full">
    <div class="w-3/4">
      <span class="mt-2 text-xl text-white block text-left">Recipient</span>
      <input
        v-model="recipient"
        type="text"
        placeholder="Enter recipient"
        class="mb-3 border rounded-md w-full bg-gray-800 bg-opacity-50 text-white p-2"
      />
      <span class="mt-2 text-xl text-white block text-left">Amount</span>
      <input
        v-model="amount"
        type="number"
        placeholder="Enter amount"
        class="mb-3 border rounded-md w-full bg-gray-800 bg-opacity-50 text-white p-2"
      />
    </div>
    <Button @func="Transfer">Transfer</Button>
  </div>
</template>

<script setup>
import highlayer from "highlayer-sdk";

const wallet = useWallet();

let recipient = ref("");
let amount = ref(0);

let SigningClient = new highlayer.SigningHighlayerClient({
  sequencer: "http://sequencer.highlayer.io/",
  node: "https://seed-node.highlayer.io/",
  signingFunction: function signer(transaction) {
    let transactionHash = new highlayer.HighlayerTx(transaction).rawTxID();
    return wallet.value.signMessage(transactionHash);
  },
});

async function Transfer() {
  let address = (await wallet.value.getAccounts())[0];

  console.log(address);
  const transaction = new highlayer.TransactionBuilder()
    .setAddress(address)
    .addActions([
      highlayer.Actions.transfer({
        amount: highlayer.HiToAlan(amount.value),
        recipient: recipient.value,
      }),
    ]);

  let fee = await SigningClient.getTransactionFee(transaction);

  transaction.setActions([
    highlayer.Actions.allocateGas({ amount: fee.gasNeeded, price: 1 }),

    highlayer.Actions.transfer({
      amount: highlayer.HiToAlan(amount.value),
      recipient: recipient.value,
    }),
  ]);

  console.log(await SigningClient.signAndBroadcast(transaction));
}
</script>
