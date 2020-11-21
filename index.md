---
layout: default
---

[![bitcoinerheader](https://raw.githubusercontent.com/BitcoinQnA/ln-guide/master/assets/images/LNG7.png)](https:/ln.guide/)

## About

This page aims to help people understand a little more on the mechanics of Lightning and how it interacts with the underlying Bitcoin network and we aim to achieve this without getting too far into the technicals, although there will be some further reading [linked](#other-resources) for those that want to take things a step further. Thanks to the developers of the various applications that interact with Lightning, many of the concepts outlined here are obfuscated away behind simple user interfaces. However, it always pays to have a good base level of understanding to help help a user be more aware of what is happening when they send or recieve a payment on Lightning. 

<br/>

## Table of Contents 

1.  [The Lightning/Bitcoin stack](#the-lightning/bitcoin-stack)
2.  [Channels](#channels)
3.  [Transactions](#transactions)
4.  [Drawbacks](#drawbacks)
5.  [Wallets + Nodes](#wallets-+-nodes)
6.  [Tools](#tools)
7.  [Other resources](#other-resources)

<br/>


## The Lightning/Bitcoin stack

The Bitcoin 'base layer' (the blockchain) cannot facilitate enough transactions to allow billions of people to use it every day. For example, if a couple of million people suddenly wanted to start using the network daily to everyday purchases, the transaction queue and fee rate would quickly spike as people compete to get their transactions processed. To aleviate this and enable the network to scale to cope with the expected exponential increase in transaction numbers, a layered system is being worked on and rolled out, much like the [internet protocol stack](https://en.wikipedia.org/wiki/Internet_protocol_suite).

Of course these transaction levels could be drastically increased by using a centralised database, similar to the current financial system with have today. But that wouldn't be very 'Bitcoin', would it? Enter the original 'layer 2' solution, [The Lightning Network](https://lightning.network/lightning-network-paper.pdf).

The Lightning Network is scaling solution built on top of the Bitcoin protocol. It facilitates smaller, near instant payments between users at very low cost. It prevents the need for every transaction made to take place on the Bitcoin ‘base layer’ whilst still ensuring that the value being transacted abides by the rules of the Bitcoin network. It is trustless, with no centralised databases and every part of the Lightning Network starts from and finishes up, on the Bitcoin blockchain. Users can exit 'layer 2' and return to the base layer at any time they like. 


## Channels

What are channels?
2 of 2 Multi-sig
Channel closures (3 types)
Who to open one with?


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

##  Wallets + Nodes

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
