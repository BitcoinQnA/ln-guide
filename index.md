---
layout: default
---

[![bitcoinerheader](https://raw.githubusercontent.com/BitcoinQnA/ln-guide/master/assets/images/LNG7.png)](https:/ln.guide/)

## About

This page aims to help people understand a little more on the mechanics of Lightning and how it interacts with the underlying Bitcoin network and we aim to achieve this without getting too far into the technicals, although there will be some further reading [linked](#other-resources) for those that want to take things a step further. Thanks to the developers of the various applications that interact with Lightning, many of the concepts outlined here are obfuscated away behind simple user interfaces. However, it always pays to have a good base level of understanding to help help a user be more aware of what is happening when they send or recieve a payment on Lightning. 

<br/>

## Table of Contents 

1.  [The Lightning/Bitcoin stack](#the-bitcoin-and-lightning-stack)
2.  [Channels](#channels)
3.  [Transactions](#transactions)
4.  [Drawbacks](#drawbacks)
5.  [FAQ](#faq)
6.  [Tools](#lightning-tools)
7.  [Other resources](#other-resources)

<br/>


## The Bitcoin and Lightning stack

The Bitcoin 'base layer' (the blockchain) cannot facilitate enough transactions to allow billions of people to use it every day. For example, if a couple of million people suddenly wanted to start using the network daily for everyday purchases, the transaction queue and fee rate would quickly spike as people compete to get their transactions processed. To aleviate this and enable the network to scale to cope with the expected exponential increase in transaction numbers, a layered system is being worked on and rolled out, much like the [internet protocol stack](https://en.wikipedia.org/wiki/Internet_protocol_suite).

Of course these transaction levels could be drastically increased by using a centralised database, similar to the current financial system we have today. But that wouldn't be very 'Bitcoin', would it? Transactions could be censored, privacy would be non-existant and we would be no better off. Enter the original 'layer 2'  Bitcoin solution, [The Lightning Network](https://lightning.network/lightning-network-paper.pdf).

The Lightning Network is scaling solution built on top of the Bitcoin protocol. It facilitates smaller, near instant payments between users at very low cost. It prevents the need for every transaction made to take place on the Bitcoin ‘base layer’ whilst still ensuring that the value being transacted abides by the rules of the Bitcoin network. It is trustless, with no centralised databases and every part of the Lightning Network starts from and finishes up, on the Bitcoin blockchain. Users can exit 'layer 2' and return to the base layer at any time they like. 

<p align="center">
<img src="https://raw.githubusercontent.com/BitcoinQnA/ln-guide/master/assets/images/LN%20visual.png" class=responsive width="750" height="350" maxheight="300" />

A visualisation of the Lightning Network by <a href="https://explorer.acinq.co/">Acinq</a>
</p>

<br/>

## Channels

The Lightning Network consists of thousands of two party payment channels. These payment channels enable those two users to pay one another back and forth as many times as they like, almost instantly and with no blockchain fees. 

A channel is opened by one or two users locking up an amount of sats into an on chain 'funding transaction' that creates a 2 of 2 multi-signature wallet on the Bitcoin network, with each user receiving one of the keys. The opening channel 'state' will reflect the amount each user contributes and each party will sign off to say that they accept this is correct. This 'sign off' is actually an unbroadcasted Bitcoin transaction that contains the signature of both parties which are passed to one another over the Lightning network. 

These signed but unbroadcasted transactions allow either party to close the channel at any point and ensures the sats contained within are returned 'on chain' to their rightful owner. Each time a payment is made from person A to person B and vice versa over Lightning, the two parties will sign a Bitcoin transaction to reflect the updated balance of each party and then pass the siged transaction to their counter party. This process can be repeated an unlimited amount of times and these signed transactions are only ever broadcast to the Bitcoin network in the event of a channel closure. 



<p align="center">
<img src="https://raw.githubusercontent.com/BitcoinQnA/ln-guide/master/assets/images/Channel%20open.png" class=responsive width="650" height="400" maxheight="300" />
  
Simple illustration of a channel open transaction
</p>


### Channel Closures

Much like a channel open, a channel closure is an on-chain Bitcoin transaction. Channel closures occur when one or both parties want to settle their balance back to the Bitcoin network. There are three types of channel closures that can occur with Lightning.

**Collaborative close**

Where both parties agree to close the channel and the most recent state is broadcast to the network.

**Force Close**

Where one party closes the channel without the consent of their counterpart. These types of closures generally occur when one of the channel parties is unreachable. For a force close to take place, one user simply broadcasts the most recent channel state known to them. Once a force close is confirmed onto the blockchain, the user that initiated the force close will have their balance locked for a set amount of time. This enables their counterparty to recognise the channel close and dispute it if they do not agree with the outcome.

**Cheat close**

A cheat close is the same as a force close, except that the initiating party is publishing an old channel state that favours them and pays them more sats back on chain. The protocol is well structured to penalise this sort of behaviour, provided your hardware is online around the time that the cheat closure transaction is broadcast.


### Who to open a channel with?

You can open a channel with pretty much any network participant you like, however there are a number of things to consider before doing so...

* **Is it someone you are likely to be transacting with often?** If they are, it makes sense to have a direct channel open to minimise routing fees.
* **Are they a reliable peer?** If they are offline regularly this will cause you issues when sending or receiving transactions.
* **Are they trustworthy?** As mentioned above, your peer has the option to attempt to cheat you when closing a channel.
* **Are they well connected?** This will become clearer in the next section on routing.

You can compare these stats and many more when choosing a peer at [1ml](https://1ml.com/).

<br/>

##  Transactions

Being able to do fast and cheap payments to a single user might be beneficial if you are conducting many transactions but don't forget that each channel has a blockchain footprint of two on chain transactions, so opening a channel to do just a couple of payments is counter productive. Lightning is not always the answer. So what happen's when a user wants to make a Lightning payment to someone they don't have a channel open with, surely they don't need to have to open channels with every person they want to transact with? 

Thankfully not, this is where the Lightning Network starts to shine! Provided you have 1 or 2 channels to fairly well connected nodes, you can route transactions to people you aren't directly connected with, via people you have a direct connection (a channel) with.

<p align="center">
<img src="https://raw.githubusercontent.com/BitcoinQnA/ln-guide/master/assets/images/Tx%20route%202.png" class=responsive width="650" height="380" maxheight="300" />
  
Alice routing a payment to Dan, despite not having a direct channel with him.
</p>

This type of multi-hop transaction is carried out in a trust free way using a process called [onion routing](https://wiki.ion.radar.tech/tech/lightning/onion-routing). This method allows for secure transfer of 'messages' known as Hashed Time Locked Contracts or [HTLC's](https://en.bitcoin.it/wiki/Hash_Time_Locked_Contracts) are structured in such a way that each 'hop' only sees the information they need to take their fee and continue the payment to the next participant in the route until it reaches the final destination, the recipient.

Here is a simplified run down of what happens when Alice presses 'send' in her Lightning wallet for the transaction above. It sounds complicated, but it all happens under the hood in a matter of seconds.

1.  Dan sends Alice an invoice (a payment instruction) for 10,000 sats. This is generated by Dan's Lightning wallet and can be sent to Alice via any communication method
2.  Alice receives the invoice and opens it with her Lightning wallet. She checks the details and confirms the send
3.  Alices Lightning node uses it's knowledge of the Lightning network to look for a suitable path to route the payment to Dan
4.  Once a route is found, the initial hop goes to Bob, whom Alice has a direct channel with
5. Alice's message to Bob says that if he sends 10,001 sats to Carol, he can keep 1 sat for himself
6. They update their channel state
7. Bob send a message to Carol to say that if she sends 10,000 sats to Dan, she can keep 1 sat for herself
8. They update their channel state
9. Carol sends a message to Dan, which Dan has a secret to unlock, allowing him to claim the 10,000 sats from Alice
10. They update their channel state

*There are no 'real world' identities in Lightning, the names above are purely for illustration purposes. Each participant has a Node Public Key which acts as their 'Lightning ID'.*

<br/>

##  Drawbacks

Whilst the Lightning Network provides a fantastic scaling solution for Bitcoin, it does not come without its own limitations. These are outlined breifly below...

* **Channel management** - If a user makes a lot of payments in a single direction, channels can become unbalanced, meaning that all of the funds are stuck on one side of the channel. This then requires the user to take action by balancing their channels. This can be done by circular rebalancing (paying yourself out of one channel and into another) or via a [submarine swap](https://lightning.engineering/loop/) service that allows you to drain off or fill up an existing channel for a small fee.

* **Inbound liquidity** - If a user opens a channel to someone else, all of the funds intially sit on their side of the channel. This means they can only send payments and cannot receive. Lightning wallets like [Breez](https://breez.technology/) and [Pheonix](https://phoenix.acinq.co/) have recently released upgrades that mitigate this by opening channels [on the go](https://medium.com/breez-technology/the-breez-release-candidate-getting-lightning-ready-for-the-global-takeover-b5d1f9756229) as required. However for those running their own Lightning Node and managing their own channels, there are three main ways around this...
  * Spend some sats 'away' to the other side of the channel
  * Purchase some inbound liquidity from a service like [Lightning Pool](https://lightning.engineering/posts/2020-11-02-lightning-pool/)
  * Use a submarine swap service

* **Channel Size** - If a user opens a channel for 1 million sats and then needs to make a payment of 1.5 millions sats, they cannot do so without the use of [Multi Path Payments](https://lightning.engineering/posts/2020-05-07-mpp/) which allows the use of more than one payment channel controlled by a single user to route a transaction.

* **Route liquidity** - If Alice wants to send a large payment to Dan over Lightning, she needs all of the people on her chosen route to have at least that amount of channel balance otherwise the payment will fail. This only realy becomes an issue for larger payments.

* **Hot Wallets** - Due to the nature of the Lightning network, a user's Lightning node needs to be online 24/7 to acknowledge and sign transactions back and forth. This means that it is advisable that users do not lock up large amounts of bitcoin without taking proper security and backup precautions.

* **Backups** - Bitcoin users will be used to backing up their seed phrase and Lightning wallets are no different. However, Lightning also has [static channel backups](https://wiki.ion.radar.tech/tutorials/troubleshooting/static-channel-backups#solution-2-static-channel-backups-scb) to allow users to recover their off chain Lightning funds in the event of hardware failure or something similar.

<br/>

## FAQ

Do I need a node?
Is there a lightning coin?
Why do payments fail?
What is Keysend?
What are MPP?

##  Lightning Tools

* [Wallets](https://bitcoinwallet.guide/lightning)
* [Nodes](https://node.guide)
* [Bitrefill](https://www.bitrefill.com/)
* [Lightning Network Stores](https://lightningnetworkstores.com/)
* [Network Explorer](https://1ml.com/)
* [Lightning tips](https://tippin.me/)


##  Other Resources

* [White paper](https://lightning.network/lightning-network-paper.pdf)
* [In depth Lightning Network Explainer](https://dev.lightning.community/overview/)
* [Lightning Wiki](https://lightningwiki.net/index.php/Main_Page)
* [Extensive list of Lightning resources](https://github.com/bcongdon/awesome-lightning-network)


Different implementations

Video guides

***

<p align="center">
  <a href="https://twitter.com/BitcoinQ_A">By Bitcoin Q+A</a> |
  <a href="https://btcpayjungle.com/apps/2Rj7Z4rAJhczJtVjr6B8eKAJLLy3/pos">Support</a> |
  <a href="https://github.com/BitcoinQnA/ln-guide">Create a PR</a>
  <br><br>
</p>
