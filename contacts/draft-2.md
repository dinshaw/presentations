™

# Context - 1 min
Our company provides...
We are almost 15 years old...
Monolithic Java supporting .5 million paying users with x billion contacts
Started moving to an SOA; bought Bantam...

# Brief Bantam History - 1 min
* Engine yard deploy with Capistrano
* MySQL

# Brief Architecture history - 5 min
* The problem: replace a huge, undocumented app with paying users. Build for 500k and x tps
* One page app; ui and service; consume our own api!
* So we just started building... which was the right thing to do, i.e.: do what you would normally do.
* Then we started getting fancy and added Solr
* And we TTD'd, cuz that's how we do, and we got to:
** Integration tests in ui, needed to spin up api, and solr, and, artifice/sinatra service stubs - what is solr good for?? redisS
* Getting started now included a launchctl
* Consolodate apps and swiched to Webmock (in process), thank god...
* Couch, no. Mysql, yes.
Torquebox

# Cell architecture - 2 min

# Performance investigation - 1 min
* Check in with the SLAs.
* I didn't even know we had those...
* 500 get ran at 44 seconds. SLA was 500ms, as expected...
* Assignments made to the deeper java folks for investigation

# Associations - 5 min
- First join, vs eager_load, etc
- Wrote sql
- Arel talk

# Ruby Profiling - 2 min
* Tools
* Loops

# AR Marshaling - 5 min

# DB Performance work - 10 min
* CPK
** mega morph
* Partitioning

# Other work we had to do
* Connection Pools
