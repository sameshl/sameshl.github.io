---
title: GSoC 2020 - Week 4 (Some good news too!)
date: 2020-06-28 21:52:38
tags:
---
Hi y'all!

Its been 4 weeks into the coding period of my GSoC 2020.

First off, I have some good news to share:

**The task of creation of database schema where each resource has its own table, by reading the ApiDoc on runtime has been completed!**
{% img https://i.pinimg.com/originals/f2/09/84/f20984b12874d68b5443dbbd124e22cc.gif '"HydraEcosystem" "HydraEcosystem logo"' %}

## Previous schema
{% img https://raw.githubusercontent.com/HTTP-APIs/hydrus/develop/docs/wiki/images/db_schema.png '"Hydrus Schema" "Hydus schema"' %}

## New schema
{% blockquote %}
As the new schema depends on the ApiDoc, the below schema is for this [Drone ApiDoc](https://github.com/HTTP-APIs/hydrus/blob/master/hydrus/samples/hydra_doc_sample.py).
{% endblockquote %}

{% img /images/week-4/drone_er.png 1000 1000 '"New Drone schema" New Drone schema"' %}

The ER Diagram isn't that clear because I didn't use any tool to draw it; I generated it using a tool called [ERAlchemy](https://pypi.org/project/ERAlchemy/). Though, I will update it with a better one, ASAP.

I will explain the important bits here:

- All the tables are for the resources defined in the Drone ApiDoc.
- The lines between them are foreign key constraints. These were established by checking if any of the `supportedProperty` of that resource are referencing any other resource. This could happen by either referencing them through the *property* attribute of that supportedProperty (for eg, ['vocab:State'](https://github.com/HTTP-APIs/hydrus/blob/72b4cda49ab9cfe0fb775146e8d5113f5d6869e0/hydrus/samples/hydra_doc_sample.py#L628-L633)) or through defining *hydra:Link* in the *property* attribute of that supportedProperty. The range of that *hydra:Link* would dictate the resource to which the foreign key is to be established (for eg, ['range': 'vocab:State'](https://github.com/HTTP-APIs/hydrus/blob/72b4cda49ab9cfe0fb775146e8d5113f5d6869e0/hydrus/samples/hydra_doc_sample.py#L503-L509)).
- The `State` column in the **Command** table is acting like a foreign key to **State** table.
- The `DroneState` column in the **Drone** table is acting like a foreign key to **State** table.
- The `State` column, `Data` column, `Command` column in the **LogEntry** table are acting like a foreign keys to **State** table, `Datastream` table and `Command` table respectively.

## Benefits of this new database architecture
- As this database architecture makes a table for each resource, this really improves its scalability and efficiency.
- The database operations are much more efficient now.
- The codebase (CRUD operations part) has become less complex. As now all the linking for RDF triples are done by the inherent database tables. Before, the schema was really generic, which meant, overhead on the developer's side to implement all the CRUD operations. But now, as the database has simplified to tables for each resource, this means that overhead is taken care by SQL-Alchemy. And we can get away with a very simple statement of the kind `table.insert(value)`(Thank you SQL-Alchemy).
- Fun fact: If you will observe my [PR](https://github.com/HTTP-APIs/hydrus/pull/479) for this feature, you will observe that in net total, there is actually code that is removed from the codebase! Even after implementation of such big feature. This goes to prove the fact that this feature, bringing more optimisation to *hydrus*, infact reduces its complexity.

## Learnings
It took me a little more time than I anticipated for implementing this feature as the previous code for database operation was really tighly coupled to existing schema. There was not that much abstraction between the under the hood database queries and the higher level logic for parsing data.

This taught me how to write better decoupled code so the project becomes maintainable on a longer run.

Also, after completing this feature, I have become confident of ~97-99% of the *hydrus* codebase. As during implementation, I faced a lot of bugs in between which lead to many `pdb` sessions through which I navigated through deep stack traces and essentially going over a major chunk of the hydrus codebase, *line by line*.

I also faced many bugs in between and after spending some time trying to debug, Python's *pass by reference* hit right on my face (facepalm!)

All the work I have done for this feature is in this [PR](https://github.com/HTTP-APIs/hydrus/pull/479).

## Path Ahead

The Phase 1 evaluations are coming up tomorrow. Fingers crossed.

Also, the next problem we would like to tackle is the [Treating collections as a resource](https://github.com/HTTP-APIs/hydrus/issues/416) in *hydrus*. Looking forward towards going about it!