---
title: GSoC 2020 - Week 12
date: 2020-08-26 18:38:19
tags:
---
This was the last week of my GSoC 2020. The main focus for this week was on completing any kind of left over work from previous phases, removing any bugs and adding documentation, basically, to get our work polished and ready for the next release of `hydrus`.

## Multiple resource type collections
Majority of my time in the last two weeks was spent on adding support for [Multiple resource type collections](https://github.com/HTTP-APIs/hydrus/issues/489). 
After PR for [Treating Collection as a resource](https://github.com/HTTP-APIs/hydrus/pull/488/) got merged, `hydrus` had support for treating collections as a resource.
But in that implementation, `collections` were restricted to only a collection of a single type class.

Also, as described in the [spec](https://www.hydra-cg.com/spec/latest/core/#collections), collections are described as a '*set of somehow related resources*'. This would also mean that a collection might also be a set of classes of different types.

We need to add support for this type of collections too.

The support for this feature was added in [this](https://github.com/HTTP-APIs/hydrus/pull/493) PR.
##### Example usage after changes made
1) **PUT** on `/collection/`
The request body should have the list of `@id`s of class instances which would be grouped into a collection.
**NOTE: These instances should exist in the database before grouping them into a collection.**
For eg,
```json
{
  "@type": "LogEntryCollection",
  "members": [
    {
      "@id": "/serverapi/Drone/4ff14a9e-9cd0-4e8a-9c11-86ac9bec211f",
      "@type": "Drone"
    },
    {
      "@id": "/serverapi/LogEntry/aab38f9d-516a-4bb2-ae16-068c0c5345bd",
      "@type": "LogEntry"
    }
  ]
}
```
In the above example, a `LogEntry` instance with id(primary key) 'aab38f9d-516a-4bb2-ae16-068c0c5345bd' and a `Drone` instance with id(primary key) '4ff14a9e-9cd0-4e8a-9c11-86ac9bec211f' already exist in the database.
This adds data in the 'LogEntryCollection' table.
##### Example response:
```json
{
    "@context": "http://www.w3.org/ns/hydra/context.jsonld",
    "@type": "Status",
    "description": "Object with ID 50ed3b68-9437-4c68-93d4-b67013b9d412 successfully added",
    "statusCode": 201,
    "title": "Object successfully added."
}
```

2) **GET** on `/collection/id`
##### Example endpoint: `/LogEntryCollection/50ed3b68-9437-4c68-93d4-b67013b9d412`
##### Example response:
Will return all the members belonging to the collection with given `collection_id`. The members returned are all `hydra:Links`.
```json
{
  "@context": "/serverapi/contexts/LogEntryCollection.jsonld",
  "@id": "/serverapi/LogEntryCollection/50ed3b68-9437-4c68-93d4-b67013b9d412",
  "@type": "LogEntryCollection",
  "members": [
    {
      "@id": "/serverapi/LogEntry/aab38f9d-516a-4bb2-ae16-068c0c5345bd",
      "@type": "hydra:Link"
    },
    {
      "@id": "/serverapi/Drone/4ff14a9e-9cd0-4e8a-9c11-86ac9bec211f",
      "@type": "hydra:Link"
    }
  ]
}
```

## Other work
Other than working on the above feature, I spent fixing a minor bug in `hydrus`.
The PR for this fix is [here](https://github.com/HTTP-APIs/hydrus/pull/496).


## Learnings / Work done
I became more adept using the tools in the HydraEcosystem.

My PRs in this period:

- Add support for multiple resource type collections - PR [#493](https://github.com/HTTP-APIs/hydrus/pull/493)
- Fix docker bug in `hydrus` - PR [#496](https://github.com/HTTP-APIs/hydrus/pull/496)

## Next Steps
As, this week marks the culmination of the period of my Google Summer of Code 2020 journey, it was quite emotional for me.
I had one of the best experiences of my life contributing to HydraEcosystem.org during this period. I learnt a lot in the last 4 months of GSoC. From working as a team to collaborating on open source software.

I have also been added the HydraEcosystem organisation. I will use this opportunity to keep contributing to HydraEcosystem[and other open source projects too;)] even after my GSoC 2020 period. I have talked to with my mentors on the other interesting stuff they are working in the organisation and how I could possibly start working on them.

To sum it up, I think this is a **start** to my journey in open source software rather than end to my GSoC period.
{% img https://i.pinimg.com/originals/f2/09/84/f20984b12874d68b5443dbbd124e22cc.gif '"HydraEcosystem" "HydraEcosystem logo"' %}

Happy contributing, y'all!
