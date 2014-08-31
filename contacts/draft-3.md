# Intro - 30 sec
Hi my name is ...
Thanks for coming to Meet the SLAs. The learning tha i'm hoping to share during this talk falls mostly under Rails performance tuning, but there is on general lesson about building for scale, from scratch, that maybe more valuable to me than all the scaling tricks, and that is @don't over architect, or prematurely engineer. In other words don't solve problems that you don't have, even if you "know" you are going to need it later. You never know which way things are going to go, especially with large projects with lots of dependencies.
To illustrate this, A short summary of our path to the architecture that is now in production starts with four people with a lot of start-up-scale experience trying to figure out where you start when designing for 500k users. We decided on a single page ui-app, and a separate api service. We would consume our own API, thereby driving the development of the API with our own requirements, and eveything would be perfect. And, since we were going straight to and from json, a document store made way more sense than a relational db, so we started writting an ActiveModel esq wrapper for CouchDB. Sounds like a good idea, turns out to be a huge pain in the ass. And it got worse before it got better. THe first pain point was the integration tests. We do tdd, so eventually we got around to integration tests. But when you have separate ui and api, guess what, you can't run your integartion tests with plain old rspec anymore, because you need both services up. You may say "just stub your api" but when both UI and service are under heavy dev, keeping our stubs in sync with our app would have been a full time job. So we added some docs saying pull down both repos, start a api server on 3001, and run the specs.
Not toooo bad... yet.
Dev went on, and eventually we swiched from couchDB to Postgres.
So then a requirement for search arrived on our jira board and, like responsible developers building for heavy use, we added a search engine, Solr. Solr has fairly heavy config and needs to run as a service so...
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

Rails Performance
- Ruby profiling
* which tools?
* What did we find
-- db issues
-- app code issues

* DB
- CPK
-- mega-morphic
- Partitioning
* Found bottlenecks
-- Associations
-- CPK




-
80% quote.

THis talk is about my experience building a Rails app that had to support 500k paying users at launch. A volume that your average rails app never hit in their lifetimes.
Looking back on this three year journey that was the acquisition of the start-up I was contracting at to 100% of traffic being routed to our new app, I can loosely classify my learning under two headings :  Rails performance, and Building For Scale.

