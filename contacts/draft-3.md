# Intro - 30 sec
Hi my name is ...
Doing rails for ...
Thanks for coming to Meet the SLAs.
Most of what i'm going to talk about in Rails preformance but one thing about building for scale
Don't do it
To illustrate this, A short summary of our path to the architecture
single page ui-app, and a separate api service.
going straight to and from json; started writting an ActiveModel esq wrapper for CouchDB.
Sounds like a good idea- pretty proud of ourselves and we talked about contribueting back to the community and a lot of other stuff that never happened
integration tests.
stubbing not reaolistic
rails s -p 3001 -e test

Dev went on and we got a call from the database team, who said "wtf?"

Search requirement
Solr

Now we added to the docs that you had to run a rake task to intsall the solr config, and then fire that up too.

Then we started putting our real integrations in place and these did need to be stubbed, but again, because the integration suite ran in the ui code base, in-process stubs would not be available to the api, so we stood up a sinatra endpoint, and used artifice to redirect the api to stubs.
We then we had to expose an api endpoint to do data setup.
At this point we added solr to LaunchAgents so it was kind of set-it-and-forget-it. So since we already had a dependancy for a java server, we wared up our service stubs and made them a java service too. So to review : to run the test suite we needed the app, the api, solr, the stubs all wired up and talking to each other correctly.
Then we started on-boarding people and the error in our ways became very clear.
Then one day we came into work, consolodated the two apps, put the stubs in the app with webmock, and threw out solr in favor of straight db queries, which, it turns out are way faster anyway.

So there we were back with an almost out-of-the-box Rails setup, and finally we got back on the golden path of easy-ish app development. THe moral of this story is that applications are going to get complex on their own, if they do anyhting at all, and more so if tey have to do it at scale. SO keep it simple.

[costco shopping cart full of mayo?]

So architecture was chose and dev proceeded, and eventually we checeked in with our SLAs.
44 seconds -> 500ms

- Ruby profiling vs Dtrace

DB issues:

  http://blog.bigbinary.com/2013/07/01/preload-vs-eager-load-vs-joins-vs-includes.html

  Preload
  Preload loads the association data in a separate query.
  Includes
  Includes loads the association data in a separate query just like preload.
  Eager load
  eager loading loads all association in a single query using LEFT OUTER JOIN.

Ended up hand-rolling, which, thanks to RailsConf talk, can be done with arel

  https://www.youtube.com/watch?v=ShPAxNcLm3o

Cut in half ! still 24p seconds...

DATABASE

- CPK
- anti-pattern in rails?
https://github.com/composite-primary-keys/composite_primary_keys

- Multiple-column index vs multiple indexs?
https://engineering.eventbrite.com/optimizing-a-table-with-composite-primary-keys/
http://www.percona.com/blog/2014/01/03/multiple-column-index-vs-multiple-indexes-with-mysql-56/

- Partitioning
Cell architecture - Verify your partitinos??

How long now??

- Megamorphic
https://github.com/composite-primary-keys/composite_primary_keys/blob/master/lib/composite_primary_keys/relation.rb
- Things that invalidate cache - https://github.com/charliesome/charlie.bz/blob/master/posts/things-that-clear-rubys-method-cache.

So instead of extending the relation, we replaced it with a new, name-spaced ActiveRecord::Relation

Down to 12ms! for the db.... 4seconds for the query...

Contacts selector
250ms

Serialization
Don't use activer record
Use postgres! - http://reefpoints.dockyard.com/2014/05/27/avoid-rails-when-generating-json-responses-with-postgresql.html

-
80% quote.

THis talk is about my experience building a Rails app that had to support 500k paying users at launch. A volume that your average rails app never hit in their lifetimes.
Looking back on this three year journey that was the acquisition of the start-up I was contracting at to 100% of traffic being routed to our new app, I can loosely classify my learning under two headings :  Rails performance, and Building For Scale.

