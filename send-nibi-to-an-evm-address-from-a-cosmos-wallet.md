---
description: A quick guide on how to send NIBI from Cosmos wallets to an EVM wallet.
---

# Send NIBI to an EVM address from a Cosmos wallet

Cosmos wallets (e.g. Keplr, Leap, Cosmostation Wallet, etc.) and EVM wallets (e.g. MetaMask, Phantom, Coinbase Wallet, etc. ) have one address for each VM. So if I want to send NIBI from my Leap to my MetaMask, unfortunately, I can't simply input my 0x... address into the send field on Leap. I need to input the NIBI address corresponding to my MetaMask which you can find [here](https://app.nibiru.fi/portfolio) after connecting you wallet.&#x20;

<div align="center" data-full-width="true"><figure><img src=".gitbook/assets/image (6).png" alt="" width="292"><figcaption><p>Copy the NIBI Address from this window to <br>send from Leap to MetaMask </p></figcaption></figure></div>

<figure><img src=".gitbook/assets/image (5).png" alt="" width="291"><figcaption><p>Copy the ETH Address from this window to <br>send from MetaMask to Leap </p></figcaption></figure>

So when I sent NIBI to `nibi1g3luq3cltkq4pfusuxgqpycppzkvuz78xffsyt` from my Leap wallet, `0x447fC0471f5d8150A790e19000930108aCCe0BC7` will receive it in my MetaMask.&#x20;

Note: This process gets flipped when sending funds from MetaMask (EVM) to Leap (Cosmos).&#x20;
