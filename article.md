# From Solr to Elasticsearch — A story by TheFork

A long time ago  in a company far far away…

Actually it was a few months ago and TheFork is already based in a lot of countries, so it should not be that far away.
## What do we need to search for?
As you might know (or maybe not), TheFork allows for a customer to book a restaurant. So we have these two related entities: **Reservation** and **Customer**

We use a microservice architecture, so each of these entities live on their own microservice.

But if we want to seach for a customer or a reservation, we want to display them as fast as possible, so not from a database but from our Solr instance.
## The old ways (Solr)

Why did we want to move away from Solr? It was not the technology itself that had issues, but we had a lot of new functional needs and a desire to start fresh.

We had a lot of unneeded fields on our index that were just for information retrieval. We already had this fields on our database if we were not searching for them, we did not need this duplicated data.

So we wanted a fresh new index with only the needed fields and a fresh new technology.
## Learning with Elasticsearch

After some technology discovery from our architecture team, the decision was to go with Elasticsearch.

But we had an issue, we had no one with knowledge on this technology. And most of the developers that worked on the previous search solution had also already left the company.
