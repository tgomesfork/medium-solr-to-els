# From Solr to Elasticsearch — A story by TheFork

A long time ago in a company far far away…

Actually it was a few months ago and TheFork is already based in a lot of countries, so it should not be that far away.
## What do we need to search for?
As you might know (or maybe not), TheFork allows for a customer to book a restaurant. So we have these two related entities: **Reservation** and **Customer**

We use a microservice architecture and each of these entities live on their own microservice.

When a restaurant user wants to search for a customer or a reservation, we want to display them as fast as possible, so certainly not from a database but from a dedicated search instance.
## The old ways (Solr)

We had an instance of SolR indexing customers and reservations everytime the user created a reservation or added a new customer.

So why did we want to move away from Solr?

It was not the technology itself that had issues, but we had a lot of new functional needs and a desire to start fresh. Most of the developers that worked on the previous search solution had already left the company, the current solution and the decisions behind were not clear anymore for the rest of us.

For instance, we had a lot of unneeded fields on our index just for information retrieval. We already had these fields on our database and if they were not searchable, we did not need to duplicate them. Why were those fields there?

The feature itself was not working perfectly, it took something like 3 minutes to index a customer or reservation. Just imagine the restaurant user adding a reservation and having to wait 3 minutes to search for it! Definitely we had to improve this...

So we dreamed of a brand new index with only the essential fields and a fresh new technology to learn and on top of this we had one major goal: be faster.
## Learning with Elasticsearch

After some technology discovery from our architecture team, the decision was to go with Elasticsearch.
- it is widely used
- it feats our docker/aws infrastructure
- insert and improve more reasons

But there was a tiny issue: we had no one with knowledge on this technology and our knowledge on the current behaviour of the project was very little. Ah, we were almost forgetting, we had 1 month to complete the project.

First thing we did, how surprising, was to study through online courses the 101 on elasticsearch. We learnt we had to create an index to store documents following a mapping we would have to define. Then we would have to define the search to retrieve relevant documents from this index.

As soon as we could understand the basics, we tried the hands dirty approach and we sketched a first mapping. As we said, we wanted a slim approach: only store fields that the search will use, do not store any information not used for the search. We came to quite simple mapping, 6 or 7 fields, only one nested type.

Now, what search should we built? Well, to answer this we defined some relevance rules. We typed some examples and defined what should be the order of the results. Once having this specification, we started to work on a search that gave us the documents we searched for in the correct order, just as we had previously defined.

We did not have much trouble nailing it. It was mainly about understanding the type of queries, how to search within nested types, ngrams topic, how to match either prefix or full names, score with the right gauss function, play around with some weights. We were doing good.

Then eventually we did some cheating. We scheduled a meeting with people outside the company that could help us improve what we already had. We did some prep: we wrote a brief documentation explaining our decisions on both index and search and delivered it to them before the meeting. This saved us sometime in a very well-paid meeting...
Surprise, surprise, or maybe not, they found some mistakes in our approach. We took the time not only to discuss our mistakes but also to learn on mechanisms of elasticsearch that either we didn't know about or had just some vague notion of how it worked. So it was not only about finding issues and fix them or finding more suitable solutions, it was also about validating and having confidence on what you were building. Like a peer review. So if you can afford it totally go for it. Return of time invested: 5/5.

Then we explored the options that were opened to us since the meeting, did some improvements not only in the search but also in the mapping itself. Next step was to simulate the real environment and see how the search played.

Ok, we admit, we had some improvements over solr, but we were far from being happy with it and we felt a little bit discouraged by these results.

// insert results?

Introspection phase, we had to think about what was happening.

Remember we said we didn't want to duplicate data into our elasticsearch index if it was not searchable? Well, we ended up doing it for some of the fields. Why?

Our search results are displayed in a list format and although some fields are not searchable they are fetched from the database and displayed in this list. So we found out that wasn't elasticsearch that was being slow, but rather this fetching from databases, which live across other microservices, that was slowing everything down. So to fix this, we had to include this data into our index, even if not searching for it. We were pretty selective about it, so we ended up with more 2 or 3 fields in the end. Still, nothing compared to the big mapping we had on solr previously.

Yes, it was not only this... we also fixed some more flaws on our search, but all of them quite expectable for newbies and none of them worth mentioning.

## Results

We had one performance **Key Result** that stated *"Customer searches calls are returned in less than 300 ms vs 500 ms today."*

So, were we able to achieve this?

We completely overachieved this! We were able to get to a median call time of **19ms**!

We can see some more details here of our new customer search call times:

|     | max    | avg |
| --- | ------ | ------ |
| **p25** | 18 ms  | 15 ms  |
| **p50** | 25 ms  | 19 ms  |
| **p75** | 58 ms  | 26 ms  |
| **p95** | 229 ms | 115 ms |
| **p99** | 969 ms | 180 ms |

We can see that this bet was a total success!

We went from knowing nothing about how our search implementation worked, to completelly changing our search infrastructure to a different technology and improving its performance greatly.

## What did we learn?

From working on this subject we should conclude that we should not be afraid of change. We can challenge ourselves to achieve better results.

We hypothesized that there was a better technology to achieve what we were already doing. We consider some aspects, we selected what we thought was most suitable fo us, we learned it and tried to apply it. We could have failed, but on this case it turned out to be the right move, and we could not be happier about it.
