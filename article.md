# From Solr to Elasticsearch — A story by TheFork

A long time ago  in a company far far away…

Actually it was a few months ago and TheFork is already based in a lot of countries, so it should not be that far away.
## What do we need to search for?
As you might know (or maybe not), TheFork allows for a customer to book a restaurant. So we have these two related entities: **Reservation** and **Customer**

We use a microservice architecture and each of these entities live on their own microservice.

When a restaurant user wants to search for a customer or a reservation, we want to display them as fast as possible, so certainly not from a database but from a dedicated search instance.
## The old ways (Solr)

We had an instance of SolR indexing customers and reservations everytime the user created a reservation or added a new customer.

So why did we want to move away from Solr?

It was not the technology itself that had issues, but we had a lot of new functional needs and a desire to start fresh. Most of the developers that worked on the previous search solution had already left the company, the current solution and the decisions behind were not clear anymore for the rest of us.

For instance, we had a lot of unneeded fields on our index just for information retrieval. We already had these fields on our database and if they ere not searchable, we did not need to duplicate them. Why were those fields there?

The feature itself was not working perfectly, it took something like 3 minutes to index a customer or reservation. Just imagine the restaurant user adding a reservation and having to wait 3 minutes to search for it! Definitly we had to improve this...

So we dreamed of a brand new index with only the essentiald fields and a fresh new technology to learn and on top of this we had one major goal: be faster.
## Learning with Elasticsearch

After some technology discovery from our architecture team, the decision was to go with Elasticsearch.
- it is widely used
- it feats our docker/aws infrastructure
- insert and improve more reasons

But there was a tiny issue: we had no one with knowledge on this technology and our knowledge on the current behaviour of the project was very little. Ah, we were almost forgetting, we had 1 month to complete the project.

First thing we did, how surprising, was to study through online courses the 101 on elasticsearch. We learnt we had to create and index to store the documents following a mapping we would have to define. Then we would have to define the search to retrieve relevant documents from this index.

// we talked to dome guru guy

As soon as we could understand the basics, we tried the hands dirty approach and we sketched a first mapping. As we said, we wanted a slim approach: only store fields that the search will use, do not store any information not used for the search.

The we defined some relevance rules. We typed some examples and defined what should be the order of the results. Once having this specification, we started to work on a search that gaves us the documents we searched for in the correct order.
