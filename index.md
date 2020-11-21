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
5.  [Wallets and Nodes](#wallets-and-nodes)
6.  [Tools](#tools)
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

A channel is opened by one user locking up an amount of sats into an on chain 'funding transaction' that creates a 2 of 2 multi-signature wallet on the Bitcoin network, with each user receiving one of the keys. The opening channel 'state' will reflect that one user is putting in x amount of sats and each party will sign off to say that they accept this is correct. This 'sign off' is actually an unbroadcasted Bitcoin transaction that contains the signature of both parties which are passed to one another over the Lightning network. 

These signed but unbroadcasted transactions allow either party to close the channel at any point and ensures the sats contained within are returned 'on chain' to their rightful owner. Each time a payment is made from person A to person B and vice versa over Lightning, the two parties will sign a Bitcoin transaction to reflect the updated balance of each party and then pass the siged transaction to their counter party. This process can be repeated an unlimited amount of times and these signed transactions are only ever broadcast to the Bitcoin network in the event of a channel closure. 

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
* **Are they a reliable peer?** If they are offline regularly this will cause you issues.
* **Are they trustworthy?** As mentioned above, your peer has the option to attempt to cheat you when closing a channel.
* **Are they well connected?** This will become clearer in the next section on routing.

<br/>

##  Transactions

Basic tx routing
HTLC
Invoices
Fees
Keysend?
MPP?

##  Drawbacks

Channel management
Inbound liquidity

##  Wallets and Nodes

See bitcoinwallet.guide + node.guide for ref

##  Tools

Wallets
Nodes
LN markets
Bitrefill
Other stores
Strike?


##  Other Resources

White paper
Video guides
Explainers
Different implementations
https://github.com/bcongdon/awesome-lightning-network
Explorers
https://lightningwiki.net/index.php/Main_Page


***

<p align="center">
  <a href="https://twitter.com/BitcoinQ_A">By Bitcoin Q+A</a> |
  <a href="https://btcpayjungle.com/apps/2Rj7Z4rAJhczJtVjr6B8eKAJLLy3/pos">Support</a> |
  <a href="https://github.com/BitcoinQnA/ln-guide">Create a PR</a>
  <br><br>
</p>
