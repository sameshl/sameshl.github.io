---
title: GSoC 2020 - Week 6
date: 2020-07-13 15:33:56
tags:
---
So, six weeks have elapsed of my GSoC 2020. It feels great to be working on the project as we have made a good amount of progress.

Also, the first evaluations had been completed and I had successfully passed them. That's good!

Now coming to what I have been working on the last two weeks. Initially the plan was to start working on the [Treating collections as a resource](https://github.com/HTTP-APIs/hydrus/issues/416) issue in `hydrus`.

But after discussion with my mentors, they suggested me to work on two smaller issues before tackling the above issue as that is a big one.

# The problems

**1. [Use of "*Collection" as default notation for Collection resources](https://github.com/HTTP-APIs/hydrus/issues/481)**

In a lot of places `hydrus` assumes that the collection for a certain resource will be of the name "[Classname]Collection". This has been added in a lot of places in the code as well(hardcoded). For example [here](https://github.com/HTTP-APIs/hydrus/blob/72b4cda49ab9cfe0fb775146e8d5113f5d6869e0/hydrus/data/crud.py#L132).

Ideally, Collection endpoint and resource can use any "name" and we should not limit it to the current format.

#### Solution

Collections have to be uniquely identified from the APIDOC. Also, the parsing of resources from the API Doc is actually taken care by the [`hydra-python-core`](https://github.com/HTTP-APIs/hydra-python-core) module. So, the actual problem here lies in that module rather than `hydrus`.

After going through the hydra specs, we understood that collections in an APIDOC can be identified through two ways.
- All collections have to have an inline ['manages'](https://www.hydra-cg.com/spec/latest/core/#manages-block) block. The manages block is a way to add information about the members of that collections.
- All collections can be provided in the `hydra:collection` property of the EntryPoint. 

Both of these cases can be understood from [this](https://github.com/HydraCG/Specifications/blob/master/drafts/use-cases/1.entry-point.md#details) example.

The work I have done is in this PR: https://github.com/HTTP-APIs/hydra-python-core/pull/41

There was also complementary work done by in [this](https://github.com/HTTP-APIs/hydra-python-core/pull/40) PR by another fellow GSoC participant, Priyanshu.

**2. [hydrus hardcoded with 'vocab:' to check foreign key relationship](https://github.com/HTTP-APIs/hydrus/issues/482)**

At the moment, hydrus has been hardcoded with `vocab:` while it tries to identify foreign key relationship. For eg, [here](https://github.com/HTTP-APIs/hydrus/blob/27db73939f2fe8a8d08a8e3c2096876247dcf7d0/hydrus/data/db_models.py#L90).

But, we should not hardcode the method to detect any kind of relationships.
We(in `hydrus`) should actually expand the `vocab:` keyword and also check that for identifying foreign key relationship. For eg, something like https://www.markus-lanthaler.com/hydra/event-api/vocab#Class1.

But the problem here is that just expanding `vocab:` won't solve the underlying issue for discovering all foreign key relationships correctly. For example, we could as well have something called `myownvocab:` in `@context` node of the Hydra API doc and then that could be used to reference something like `myownvocab:Class1`.

We might also extract this correctly after some work at parsing the API doc. But if suppose our context becomes nested, it will become difficult to parse.

This issue is actually directly related to [this](https://github.com/HTTP-APIs/hydra-python-core/issues/38) existing issue in the `hydra-python-core` library.

#### Solution
The ideal behaviour should be that, `hydrus` starts with an expanded API doc. We should not parse the API doc without expanding it beforehand.

This change should then ideally take place it the `hydra_python_core` library, which generates the API doc object which hydrus uses for parsing data required of the API doc. Therefore the output of the `hydra_python_core` librarie's `doc_maker` module should be an API doc which is **expanded beforehand**.


For working on this, Priyanshu started with the changes in the core module in [his](https://github.com/HTTP-APIs/hydra-python-core/pull/42) PR.
I then started with doing necessary changes in `hydrus` after these changes in the core library.


### Learnings
I learnt a lot more on the Hydra Spec solving the above two issues. Both of the above issues needed a lot background understanding of the spec so we could implement the required solutions in `hydrus` and more importantly the `hydra-python-core` module.