---
title: GSoC 2020 - Week 8
date: 2020-07-28 12:07:46
tags:
---
Eight weeks have elapsed of my GSoC 2020. It feels great to be working on the project as we have made a good amount of progress.

Also, the second evaluations are coming up. Fingers crossed.

Now coming to what I have been working on the last two weeks. The last two weeks were spent in tackling a very big pending issue in `hydrus`, [Treating collections as a resource](https://github.com/HTTP-APIs/hydrus/issues/416) issue.

This was one of the biggest points in my proposal for GSoC 2020 and also the mentors wanted this issue to be fixed. A lot of improvements would be made after this is fixed.


## The Problem

 Currently in `hydrus`, the treatment of `collections` as a single resource for every `class` is wrong. Nowhere in the spec is it mentioned that a collection is a set of **all** objects for a given class. It is mentioned that a collection is "*a set of somehow related resources*". This does not imply that a collection would be only limited to class types.

 What this means essentially is that currently in `hydrus` if there is a 'collection' class which is a collection of instances of type *x*, it is just being used to store **all** elements of type x.
 
 For example, looking at this [Drone ApiDoc](https://github.com/HTTP-APIs/hydrus/blob/master/hydrus/samples/hydra_doc_sample.py), we can see there is a `hydra:Collection` with title 'DroneCollection'. And there is also a `hydra:Class` of type 'Drone'. So, currently what is happening is that whenever you add a new drone to the database(via PUT request), it gets added to the `DroneCollection` collection. Therefore, all the drones will belong under just DroneCollection. Meaning if we do a GET request at `/DroneCollection` we will get the *members* property in the response as all the drones from the database.
 But, no where in the *Hydra Spec* it is written that a `hydra:Collection` will just be the set of **all** members of that class(in this case, drones).

## Solution

The way to go is to allow users to define collections on their own. Each collection itself would have an @`id` since it is a subclass of `hydra:Resource`. We then use the collection endpoint to relate classes/properties.

For example, seeing this in the context of the Comment and Issue classes:
The advantage of using a collection is that we could then define a `CommentCollection` object, where we link all `Comment` objects for a **particular** issue. We then give that `CommentCollection` object as the property for the `Issue`.

In terms of defining it in the API documentation, it would be something like:
```json
{
        "@type": "SupportedProperty",
        "property": vocab:CommentCollection,
        "readonly": "false",
        "required": "true",
        "title": "Comments",
        "writeonly": "false"
}
```

The property would map to an instance of the `CommanCollection` class. This will be defined in the `supportedProperty` field for the definition of the `Issue` class. Like any other object, when we define the Issue object, we will add the appropriate instance of the `CommandCollection` to the `Issue` instance. So an object would be something like:
```json
{
        "@type": "Issue",
        "@id": "/api/Issues/27",
        "Issue": "....",
        "Comments": "/api/CommentCollection/32",
        ....
}
```
And then `/api/CommentCollection/32` would have:
```json
{
        "@type": "CommentCollection",
        "@id": "/api/CommentCollection/32",
        "members": [
                {
                        "@id": "/api/Comment/12",
                        "@type": "Comment"
                },
                {
                        "@id": "/api/Comment/18",
                        "@type": "Comment"
                },
                {
                        "@id": "/api/Comment/22",
                        "@type": "Comment"
                }
        ]

}
```
### Implementation details
We have planned to have a new `collection_id` column for every collection table so as to distinguish which instance belongs to which collection. We need new column as the values in this column could be repeated as a collection could have many items in it.

Apart from this column, the existing columns include `id`, which acts as the primary key for that table and `members` which would act as a link to the actual item in that item's table.

### Brief description of major changes after we start treating collection as a resource:
(Note: For below discussion, `CommandCollection` is a collection class, `Command` would be non-collection class / parsed class. The meaning of non-collection class would be any class which will not act as a 'collection' of items)
1) **GET** on `/collection/`
##### Example endpoint: `/CommandCollection/`
This fetches the data from the 'CommandCollection' table.
##### Example response:
Will return a list of *collections*.
```json
{
    "@context": "/serverapi/contexts/CommandCollection.jsonld",
    "@id": "/serverapi/CommandCollection/",
    "@type": "CommandCollection",
    "hydra:totalItems": 3,
    "hydra:view": {
        "@id": "/serverapi/CommandCollection?page=1",
        "@type": "hydra:PartialCollectionView",
        "hydra:first": "/serverapi/CommandCollection?page=1",
        "hydra:last": "/serverapi/CommandCollection?page=1"
    },
    "members": [
        {
            "@id": "/serverapi/CommandCollection/7d2cc88f-388a-43f1-80fc-0c2184de4784",
            "@type": "CommandCollection"
        },
        {
            "@id": "/serverapi/CommandCollection/50ed3b68-9437-4c68-93d4-b67013b9d412",
            "@type": "CommandCollection"
        },
        {
            "@id": "/serverapi/CommandCollection/c2e10f88-d205-41e7-aa61-338afd64f657",
            "@type": "CommandCollection"
        }
    ]
}
```
2) **PUT** on `/collection/`
##### Example endpoint: `/CommandCollection/`
The request body should have the list of ids of class instances which would be grouped into a collection.
For eg,
```json
{
    "@type": "CommandCollection",
    "members": ["aaaaa",
                "bbbbb",
                "ccccc"]
}
```
In the above example, 'aaaaaa', 'bbbbb' and 'ccccc' are the `ids`(primary key) of the instances in `Command` table.
This adds data in the 'CommandCollection' table.
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
**NOTE: The `id` returned is actually the value of the `collection_id` column in the table, not the primary key `id`.**
3) **GET** on `/collection/id`
##### Example endpoint: `/CommandCollection/50ed3b68-9437-4c68-93d4-b67013b9d412`
**Note: The `id` is corresponding to the `collection_id` column in the `CommandCollection` table, not the primary key `id`.**
##### Example response:
Will return all the members belonging to the collection with given `collection_id`.
```json
{
    "@context": "/serverapi/contexts/CommandCollection.jsonld",
    "@id": "/serverapi/CommandCollectionCollection/50ed3b68-9437-4c68-93d4-b67013b9d412",
    "@type": "CommandCollection",
    "members": [
        {
            "@id": "/serverapi/Command/aaaaaa",
            "@type": "Command"
        },
        {
            "@id": "/serverapi/Command/bbbbbb",
            "@type": "Command"
        },
        {
            "@id": "/serverapi/Command/cccccc",
            "@type": "Command"
        }
    ]
}
```
4) **POST** on `/collection/id`
##### Example endpoint: `/CommandCollection/50ed3b68-9437-4c68-93d4-b67013b9d412`
The request body should have the list of members to be updated for the given collection.
For eg,
```json
{
    "@type": "CommandCollection",
    "members": ["ddd",
                "eee",
                "fff"]
}
```
**Note: The `id` is corresponding to the `collection_id` column in the `CommandCollection` table, not the primary key `id`.**
##### Example response:
Will update all the members belonging to the collection with given `collection_id` with the members given in the request body.
```json
{
{
    "@context": "http://www.w3.org/ns/hydra/context.jsonld",
    "@type": "Status",
    "description": "Object with ID 50ed3b68-9437-4c68-93d4-b67013b9d412 successfully updated",
    "statusCode": 200,
    "title": "Object updated"
}
```
5) **DELETE** on `/collection/id`
##### Example endpoint: `/CommandCollection/50ed3b68-9437-4c68-93d4-b67013b9d412`
**Note: The `id` is corresponding to the `collection_id` column in the `CommandCollection` table, not the primary key `id`.**
##### Example response:
Will delete that collection from the table.
```json
{
    "@context": "http://www.w3.org/ns/hydra/context.jsonld",
    "@type": "Status",
    "description": "Object with ID 50ed3b68-9437-4c68-93d4-b67013b9d412 successfully deleted",
    "statusCode": 200,
    "title": "Object successfully deleted."
}
```
6) **GET**, **PUT**, **POST** and **DELETE** on any `/non-collection-class`
##### Example endpoint: `/Command/` or `/Area`
**NOTE: All of these will have the same behaviour as what would have happened before on `/CommandCollection/` endpoint**

## Learnings / Work done

Apart from again learning a lot of the codebase of `hydrus`, I learnt a lot on *Hydra Spec*.

All my work for this issue has been completed in this [PR](https://github.com/HTTP-APIs/hydrus/pull/488).