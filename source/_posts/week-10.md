---
title: GSoC 2020 - Week 10 (Another good news!)
date: 2020-08-17 17:27:40
tags:
---
I passed the second evaluation in GSoC 2020. Hurray!

Also, now 10 weeks have been completed. We are in the endgame now.

Now coming to what I have been working on the last two weeks. The last two weeks were mainly spent in tackling a very big pending issue in `hydrus`, [hydrus hardcoded with 'vocab:' to find relationships](https://github.com/HTTP-APIs/hydrus/issues/482) issue.

## The Problem
At the moment, `hydrus` has been hardcoded with `vocab:` while it tries to identify foreign key relationship. For eg, [here](https://github.com/HTTP-APIs/hydrus/blob/27db73939f2fe8a8d08a8e3c2096876247dcf7d0/hydrus/data/db_models.py#L90).

But the problem here is that just expanding `vocab:` won't solve the underlying issue for discovering all foreign key relationships correctly. For example, we could as well have something called `myownvocab:` in `@context` node of the Hydra API doc and then that could be used to reference something like `myownvocab:Class1`.

We might also extract this correctly after some work at parsing the API doc. But if suppose our context becomes nested, it will become difficult to parse.

This was a really big issue to tackle, not just because of the implementation in `hydrus` but also because there were a lot of changes required in the `hydra-python-core` repo. That is because the hard coded nature of `hydrus` was due to to the fact that initially `hydrus` and `hydra-python-core` were very tightly coupled. 
 
## Solution

The ideal behaviour should be that, `hydrus` starts with an *expanded* API doc. We should not parse the API doc without expanding it beforehand. This change should then ideally take place it the `hydra_python_core` library, which generates the API doc object which `hydrus` uses for parsing data required of the API doc. Therefore the output of the `hydra_python_core` library's `doc_maker` module should be an API doc which is expanded beforehand.

This dependency on the `hydra-python-core` made this issue a little more difficult than I initally anticipated.

## Other work
Other than working on the above issue, I spent some time adding documentation on `hydrus` [here](https://github.com/HTTP-APIs/hydra-python-core/pull/49). We all know how important good documentation is for any project, especially open source projects.

Also, there was some work needed to be done on the previous [Treating collections as a resource](https://github.com/HTTP-APIs/hydrus/issues/416) issue.
Basically, some work had to be done on **GET** request on any `/collection` endpoint [here](https://github.com/HTTP-APIs/hydrus/pull/492). This work was basically an update on the work in [this](https://github.com/HTTP-APIs/hydrus/pull/488) PR.


## Learnings / Work done

As most of the work done in this period was dependent on `hydra-python-core`, I really learnt how to work in a team, alongside fellow GSoCer at HydraEcocsystem, Priyanshu.

It was an amazing experience as together both of us hunted down all the bugs to make sure `hydrus` and `hydra-python-core` work in sync perfectly.

My PRs in this period:

- Remove hardcoded `vocab:` keyword - PR [#490](https://github.com/HTTP-APIs/hydrus/pull/490)
- Add docs on `hydrus` - PR [#49](https://github.com/HTTP-APIs/hydra-python-core/pull/49)
- **GET** request on `/collection` endpoint - PR [#492](https://github.com/HTTP-APIs/hydrus/pull/492)

## Next Steps
After PR [#488](https://github.com/HTTP-APIs/hydrus/pull/488/) got merged, `hydrus` will has support for treating collections as a resource.

But in that implementation, collections are restricted to only a collection of a **single** type class.
As described in the [spec](https://www.hydra-cg.com/spec/latest/core/#collections), collections are described as a 'set of somehow related resources'. This would also mean that a collection might also be a set of classes of different types.

We need to add support for this type of collections too.