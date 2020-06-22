---
title: GSoC 2020 - Week 2
date: 2020-06-21 14:11:47
tags:
---
So, first two weeks of my GSoC have been completed.

My project is basically on improving the Hydra server in Python called [hydrus](https://github.com/HTTP-APIs/hydrus).
My first task was to improve the existing database architecture that *hydrus* used internally while serving any ApiDoc.
This task was related to [this](https://github.com/HTTP-APIs/hydrus/issues/412) issue.

The existing schema:
{% img https://raw.githubusercontent.com/HTTP-APIs/hydrus/develop/docs/wiki/images/db_schema.png '"Hydrus Schema" "Hydus schema"' %}
This design for the database was really generic.

Our aim was to have a database multi-table architecture where different resources are stored in different tables to improve scalability and efficiency. The result we wanted to achieve was that there will be should be a table for each `hydra:Class` and `hydra:Collection` defined in the ApiDoc.

For example, if we consider the [Drone ApiDoc](https://github.com/HTTP-APIs/hydrus/blob/master/hydrus/samples/hydra_doc_sample.py), we would want a `State` table, `Drone` table and so on. The columns would be the properties defined under the `supportedProperty` for that Class or collection.
This would easily facilitate the operations on the data as for any operation such as updating a new record or deleting a old record, we would just need to call the methods provided by `Flask-SQLAlchemy` library. A lot of overhead which we had in our previous database schema would be minimised. 

For example, in the previous database schema, for a simple operation such as adding a [`Message`](https://github.com/HTTP-APIs/hydrus/blob/master/hydrus/samples/hydra_doc_sample.py) Instance, (by doing a **PUT** request to `/MessageCollection`) , we would need to do an INSERT operation in 4 tables, namely the `instance` table, `terminals` table, `graphiit` table, `graph` table. That is not that efficient.

In the new schema, there will be a separate table for storing all instances of `Message` class. This table will have a primary key column and a column with name `MessageString` (from the `supportedProperty` of `Message` class). So, to modify any instance of `Message` class, we will only need to do a single operation on this on table.

This will make our database operations much more efficient.

To achieve this feat, I had to read the ApiDoc and then make the database specific to that ApiDoc.

I had spent the first week reading on `hydrus`'s existing schema and understanding on how we are storing the data. This made it more clear to me on how I could optimize the database architecture.

In the second week, I had started implementing the logic for parsing the ApiDoc and making the tables from reading that. I have completed till making the tables "dynamically".
In the next week, I will look into how we can get 'linking' behaviour in the tables, meaning how would we connect tables with foreign keys by just parsing the ApiDoc.

The work I have done is in this PR: https://github.com/HTTP-APIs/hydrus/pull/479

### Learnings
I learnt a lot on how we go about database optimisation.

I also learnt how to "dynamically" create tables in a database using [Flask-SQLAlchemy](https://flask-sqlalchemy.palletsprojects.com/en/2.x/) library. The challange was mainly to create Python classes on *runtime*. Because all the operations on the tables will be done from SQLAlchemy classes.

Surprisingly, Python's unasumming `type` function was the backbone for making classes on runtime.
[This](http://sparrigan.github.io/sql/sqla/2016/01/03/dynamic-tables.html) blog post also came very handy.
