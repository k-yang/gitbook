# Send NIBI to an EVM address from a Cosmos wallet

For the purposes of this discussion, a Cosmos wallet means a Cosmos wallet extension (e.g. Keplr, Leap, Cosmostation Wallet, etc.) that is connected to a Cosmos chain (e.g. Nibiru Chain), and not an EVM chain (e.g. Nibiru EVM). They are different options in wallet extensions even though they are the same L1 chain under the hood. See below:

<figure><img src=".gitbook/assets/Screenshot 2025-03-20 at 2.23.00 PM.png" alt=""><figcaption></figcaption></figure>

For example, my Leap wallet could show an address of `nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl`while connected to Nibiru Chain (Cosmos), but it would show an address of `0xFaF227daD0b91C2dEBD41daE71C959EA4f95f8F8`when connected to Nibiru Chain (EVM).&#x20;

Let's say I want to send NIBI to `0x2DD37531749e1AF248720fe8F9D1A51517D9748F`. I need to convert `0x2DD37531749e1AF248720fe8F9D1A51517D9748F`to it's bech32 representation. That's a pain in the ass to do manually, so I [built a tool](https://app.kevinyang.dev/address) to convert it.&#x20;

<figure><img src=".gitbook/assets/Screenshot 2025-03-20 at 2.28.13 PM.png" alt=""><figcaption></figcaption></figure>

Now when I sent NIBI to `nibi19hfh2vt5ncd0yjrjpl50n5d9z5tajay0jnjqnn`from my Cosmos wallet, `0x2DD37531749e1AF248720fe8F9D1A51517D9748F`will receive it.&#x20;

<figure><img src=".gitbook/assets/Screenshot 2025-03-20 at 2.30.07 PM.png" alt=""><figcaption></figcaption></figure>
