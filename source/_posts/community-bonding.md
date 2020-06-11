---
title: Community Bonding Period - Google Summer of Code 2020
date: 2020-06-05 18:18:33
tags:
---

The community bonding period is basically meant for students to get to know mentors, read documentation, interact more with the organisation's community. Basically, to get you up to speed before the coding period starts.

So, for almost the past one month, I have been fixing minor bugs and writing documentation for the HydraEcosystem to get more used to the tools alongside [hydrus](https://github.com/HTTP-APIs/hydrus) in the HydraEcosystem. This helped me learn a ton about semantic web, JSON-LD, RDF, linked-data and Hydra.

What we, at Hydra, are working on is to automate REST APIs. We are working on building better Web APIs and 'smarter' clients.


## Automate? Better web APIs? Smarter clients? 
Even today, building APIs is more like an art than science. Today's clients are heavily hard-coded against specific APIs. This makes the clients very brittle, meaning when the API even changes slightly, the clients will break.

Let's take a example to understand this better.
Suppose you are using a using a football statistics API which gives stats for a particular team in the ongoing season. Right now it serves stats for only the teams playing in the English Premier League. 
They serve the stats for Liverpool at `http://myamazingfootballstats.com/liverpool`. Now, as they expand, they have started serving data even for Spanish La Liga. So now, they have changed their API and have started serving stats for Liverpool at `http://myamazingfootballstats.com/premierleague/liverpool`.
This *simple* change is going to break all the clients relying on this API for data. This will lead us to spend a lot of time to manually reconfigure all our clients to use the new API.

This is where Hydra comes into picture. Imagine, if there was a way to document our API, like all the endpoints it serves, the operations allowed on those endpoints, the format of data required on those endpoints, etc so that **machines** could understand this. Also, smart clients which could *understand* the API documentation and decide on how the data has to retrieved. We don't need to hardcode them. And also, smart servers, which given just this documentation, know how to set them up and start serving data. You don't need to do any setup in server such as setting up database, the server understands everything from the documentation. 

Hydra is a specific type of JSON Linked Data representation that was proposed by the W3C community. You can think of it as a standard 'vocabulary' between clients and servers. So, the client can understand the state of the server, the available endpoints, methods allowed on those endpoints and all the necessary information from the Hydra's API documentation. This documentation, is served at a URL, which can be identified by the client from the response of the server.

So, once the client is given the url of the server, it gets the API documentation. After that, it *automagically* find all the information regarding the endpoints the server is hosting by the api documentation which is needed to interact with the server.

Therefore, Hydra, is set of technologies that allow to design APIs in a different manner, in a way that enables smarter clients and more generic servers.


I am mainly going to work on the Hydra server called as [hydrus](https://github.com/HTTP-APIs/hydrus) and my fellow GSoCer at Hydra, [Priyanshu](https://github.com/priyanshunayan) is going to work on the client side part of Hydra, [Hydra Agent](https://github.com/priyanshunayan/hydra-python-agent).

## My Work
My time in the community bonding period was mostly spent on learning about all the tools in the Hydra Ecosystem. I learnt a lot on how the Hydra documentation works and how smart clients *understand* it. I also spent a lot of time reading the [Hydra Spec](https://www.hydra-cg.com/spec/latest/core/) which acts like the core vocabularly which makes all of this possible.

My PRs during this period which include working on improving documentation, refactoring existing code and fixing minor bugs:
* https://github.com/HTTP-APIs/http-apis.github.io/pull/78
* https://github.com/HTTP-APIs/hydrus/pull/478
* https://github.com/HTTP-APIs/hydrus/pull/476
* https://github.com/HTTP-APIs/hydrus/pull/474
* https://github.com/HTTP-APIs/hydrus/pull/472
* https://github.com/HTTP-APIs/documentation-hydrus/pull/14
* https://github.com/HTTP-APIs/http-apis.github.io/pull/79

My mentors [Akshay](https://github.com/xadahiya) and [Chris](https://github.com/chrizandr) helped me a lot with my doubts. I also spent a lot of my time, discussing about Hydra stuff with [Priyanshu](https://github.com/priyanshunayan).

## Path ahead
During the first two weeks, I plan to research and implement a [better database architecture](https://github.com/HTTP-APIs/hydrus/issues/412) for hydrus. Right now, the database architecture is very generic. We could make a lot of changes which will improve scalability and efficiency.

