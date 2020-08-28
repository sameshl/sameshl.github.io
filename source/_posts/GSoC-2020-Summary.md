---
title: Google Summer of Code 2020 Summary - Hydra Ecosystem
date: 2020-08-27 13:36:11
tags:
---
Hi there! 

I’m Samesh Lakhotia and this post aims to summarize my journey throughout Google Summer of Code 2020. It has been three months of intensive learning and a remarkable experience as a developer which was divided into three coding phases. This has been one the most productive and enriching summers of my life.
I’ll divide each phase citing its goal, small description and core PR links attached to each. I’ve also made detailed blog posts during each phase(bi-weekly) and they’re linked in the end of each section. The last part has some diverse auxiliary merged contributions done along the way.

[Our community](https://github.com/HTTP-APIs) has different tools including a working server that can be built from a Hydra Doc, a.k.a [`hydrus`](https://github.com/HTTP-APIs/hydrus), a Python Agent to work with that server, a.k.a [`hydra-python-agent`](https://github.com/HTTP-APIs/hydra-python-agent) and the core library to handle all the major parsing of Hydra Docs, a.k.a [`hydra-python-core`](https://github.com/HTTP-APIs/hydra-python-core). I and my GSoC colleague [Priyanshu Nayan](https://github.com/priyanshunayan) had a series of improvements to make, in which Priyanshu was leading Hydra Agents' and Core library changes and I was responsible for the `hydrus` side.


# [Phase 1] - `hydrus` database architecture improvements
The first phase of GSoC was marked by **intensive study and discussions**. I was in charge of optimising the existing database architecture of `hydrus`. The basic idea was to go from a **generic** database schema to one where **different type of resources are stored in different tables** to improve scalability and efficiency. To start this task, I had to get really comfortable with the existing database architecture.

Then, I had detailed discussed with the mentors on the possible architecture and then proceeded to the coding and lastly documenting it. References for this phase are as below:

#### Goal:
- Research and implement **better multiple-table architecture** - database architecture where different type of resources are stored in different tables to improve scalability and efficiency

#### Pull Requests:
- [[PR 479] Make db tables for each resource in apidoc](https://github.com/HTTP-APIs/hydrus/pull/479)

#### Extra Reference links:
- Older linked issues: [Research better multiple-table architecture](https://github.com/HTTP-APIs/hydrus/issues/412)

#### Detailed blog posts during this phase:
- [GSoC 2020 - Week 2](https://www.sameshlakhotia.tech/week-2/)
- [GSoC 2020 - Week 4](https://www.sameshlakhotia.tech/week-4/)

# [Phase 2] - Treating collections as a Resource
The second phase of GSoC consisted of mainly two new features and enhancements. These were some old issues in `hydrus` which needed to get fixed from a long time.
These included the ability to **treat collection as a resource** (and therefore, create custom collection instances) and also removing the unnecessary dependency of using **“*Collection” as default notation** for naming Collection resources.

#### Goal:
- Improve `hydrus` by adding feature of treating Collection as a resource and remove the dependency of using “*Collection” as default notation for naming Collection resources

#### Pull Requests:
- Collection as a Resource
  - [[PR 488] Treating Collection as a Resource](https://github.com/HTTP-APIs/hydrus/pull/488)
- Removing dependency of "*Collection" on naming Collection resources
  - [[PR 41] Discover collections in doc_maker.py](https://github.com/HTTP-APIs/hydra-python-core/pull/41)
  - [[PR 483] Better discover collection](https://github.com/HTTP-APIs/hydrus/pull/483)

#### Extra Reference links:
- Older linked issues: [Treating collections as a resource](https://github.com/HTTP-APIs/hydrus/issues/416) - [[DISCUSSION] Database design for treating collections as a resource](https://github.com/HTTP-APIs/hydrus/issues/487) - [Discovering Classes and Collections from the APIDOC](https://github.com/HTTP-APIs/hydra-python-core/issues/39)

#### Detailed blog posts during this phase:
- [GSoC 2020 - Week 6](https://www.sameshlakhotia.tech/week-6/)
- [GSoC 2020 - Week 8](https://www.sameshlakhotia.tech/week-8/)

# [Phase 3] - More features in `hydrus`!
As the last phase started, we had covered a considerable amount of the core goals we had and now was time to close things out with a some last additions. We wanted some more features in `hydrus` which included **removing hardcoded dependencies on vocab keyword** and adding support for **multiple resource type collections**.

#### Goal:
- Improve `hydrus` with main feature of multiple resource type collections and removing hardcoded dependency on vocab keyword and wrap up bug fixes and documentation.


#### Pull Requests:
- Removing hardcoded dependencies on vocab keyword
  - [[PR 490] Remove hardcoded dependencies on vocab keyword](https://github.com/HTTP-APIs/hydrus/pull/490)
- Adding support for multiple resource type collections
  - [[PR 493] Multiple resource type collections](https://github.com/HTTP-APIs/hydrus/pull/493)
- Other minor bug fixes
  - [[PR 492] Rectify GET on /collection endpoint](https://github.com/HTTP-APIs/hydrus/pull/492)
  - [[PR 496] FIX: failing docker build](https://github.com/HTTP-APIs/hydrus/pull/496)

#### Extra Reference links:
- Older linked issues: [hydrus hardcoded with 'vocab:' to check foreign key relationship](https://github.com/HTTP-APIs/hydrus/issues/482) - [Multiple resource type collections](https://github.com/HTTP-APIs/hydrus/issues/489)

#### Detailed blog posts during this phase:
- [GSoC 2020 - Week 10](https://www.sameshlakhotia.tech/week-10/)
- [GSoC 2020 - Week 12](https://www.sameshlakhotia.tech/week-12/)

# Auxiliary Contributions
There were a some additional contributions made along the way and during the Community Bonding that will be mentioned for consistency but were mainly auxiliary:

#### hydrus
- [[PR 478] Refactoring](https://github.com/HTTP-APIs/hydrus/pull/478)
- [[PR 476] Make the defaults same](https://github.com/HTTP-APIs/hydrus/pull/476)
- [[PR 474] docker-compose was failing with SSL error](https://github.com/HTTP-APIs/hydrus/pull/474)
- [[PR 478] Regenerate the examples/drones/doc.py](https://github.com/HTTP-APIs/hydrus/pull/478)
- [[PR 495] Add docs on database of hydrus](https://github.com/HTTP-APIs/hydrus/pull/495)
- [[PR 484] Update Docker image to use python 3.7](https://github.com/HTTP-APIs/hydrus/pull/484)

#### hydra-python-core
- [[PR 46] Add support for POST and DELETE on collection classes](https://github.com/HTTP-APIs/hydra-python-core/pull/46)

#### documentation-hydrus
- [[PR 14] Update docs to latest hydrus api](https://github.com/HTTP-APIs/documentation-hydrus/pull/14)

#### http-apis.github.io
- [[PR 79] Add docs on searching in hydrus](https://github.com/HTTP-APIs/http-apis.github.io/pull/79)
- [[PR 78] Add page on CRUD operations with hydrus](https://github.com/HTTP-APIs/http-apis.github.io/pull/78)
- [[PR 82] Add docs on collection as a resource and update previous docs](https://github.com/HTTP-APIs/http-apis.github.io/pull/82)

# Acknowledgments

The GSoC 2020 journey now comes to an end. I have to say it was personally a really important development phase for me and an experience that I’ll remember forever. I feel now that I’ve progressed as a Software Developer and also that we were able to progress and take Hydra Ecosystem development a little bit further with a group of people spread in the world.

I have also been added the HydraEcosystem organisation. I will use this opportunity to keep contributing to HydraEcosystem[and other open source projects too;)] even after my GSoC 2020 period. I have talked to with my mentors on the other interesting stuff they are working in the organisation and how I could possibly start working on them.

And of course, all of that would not be possible without the support, time and will of my amazing mentors [Chris Andrew](https://github.com/chrizandr), [Akshay Dahiya](https://github.com/xadahiya) and [Lorenzo](https://github.com/Mec-iS). Whom have been, not only this summer but for years, cultivating the Hydra Ecosystem and giving their time and effort to develop this concept and provide the opportunity for other people to join and learn. My honest thanks for the knowledge and trust you’ve shared with me, it’s something that I’ll take with me in my career to become a better professional.
I also have to give a huge shout out to my GSoC partner [Priyanshu Nayan](https://github.com/priyanshunayan) who helped me a lot whenever I was stuck debugging something, having a doubt in the Hydra spec or any help in general.

To sum it up, I think this is a start to my journey in open source software rather than end to my GSoC period.