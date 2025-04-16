---
title: "Portfolio item 1"
excerpt: "Goland p2p blockchain pharmacy backend<br/><img src='/images/blockchain.jpg'>"
collection: portfolio
date: 2021-01-15
---

One of my more successful freelance projects was creating a [p2p](https://en.wikipedia.org/wiki/Peer-to-peer) decentralized backend service for an existing pharmacy


Problem
-------

A client approached me with the need for a service that could process orders for certain chemicals/ pharmaceutical drugs and prevent tampering with orders. They specified the deployment should be written in (GoLang)[https://go.dev/] and be able to ensure that there was no way to alter/ re-process old orders. Understandably this is a valid concern based on the context, so we concluded that we would opt for a blockchain data structure for our solution. This is due to the fact that in these structures, every "block" (or data item) is reliant on the last (hence "chain"). If one item is tampered with, the chain is broken.

Solution
-------

For this project I designed a very simple web interface that I wrote in HTML/CSS/JavaScript that allowed authorized identities to create orders. The orders were then parsed by my Go backend which added "blocks" to the specified peer network, where multiple copies of the chain were held to build "trust". This solution was very simple but provided a seamless answer for the client in a lightweight fashion

Notes
-------

For the little amount I got paid for this project, I hate to think about the potential value it created for the client company... Though it was a useful learning tool in order to experiment with different data structures and alternate networking models
