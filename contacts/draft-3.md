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
SSO stubs ... artifice!!
[slide of setup]

Data setup ...
[slide of setup]

LaunchAgents...

Onboarding sux...

Consolidate apps...
Get rid of solr...

Architecture - jruby

[slide of cell]

Torquebox

No redis

Out-of-the-box Rails setup

The moral of this story is that applications are going to get complex on their own, if they do anyhting at all, and more so if tey have to do it at scale. SO keep it simple.

44 seconds -> 500ms

- Ruby profiling vs Dtrace vs Benchmarks

[Benchmark slide] - Newrelic
  def count
    CtctBenchmark.realtime "persons_controller_count" do
      render status: 200, json: ::V2Translator.count_persons(params)
    end
  end

DB issues:
  N+1 - http://guides.rubyonrails.org/active_record_querying.html#eager-loading-associations

  [domain slide] to show channel

  http://blog.bigbinary.com/2013/07/01/preload-vs-eager-load-vs-joins-vs-includes.html

  [slide]
  Preload
  Preload loads the association data in a separate query. always generates two sql
  Includes
  Includes attempts to run two queries, but switches to a 'LEFT OUTER JOIN'
  Eager load
  eager loading loads all association in a single query using LEFT OUTER JOIN.

Ended up hand-rolling, which, thanks to RailsConf talk, can be done with arel
  LEFT INNER JOINS

RailsConf 2014 -Advanced aRel: When ActiveRecord Just Isn't Enough
  https://www.youtube.com/watch?v=ShPAxNcLm3o

Cut in half ! still 24p seconds...

DATABASE

- CPK
- anti-pattern in rails?
https://github.com/composite-primary-keys/composite_primary_keys

- Multiple-column index vs multiple indexs?
https://engineering.eventbrite.com/optimizing-a-table-with-composite-primary-keys/
http://www.percona.com/blog/2014/01/03/multiple-column-index-vs-multiple-indexes-with-mysql-56/

- Megamorphic

    class Foo
      def do_something
      end
    end

    class Bar
      def do_something
      end
    end

    a = [Foo.new, Bar.new] * 1000
    a.each { |obj|
      # inline cached, eventually JITs
      obj.do_something
    }

    # inline cached code path
    case obj.class
    when Foo
      Foo.do_something
    when Bar
      Bar.do_something
    else
      lookup ...
    end

https://github.com/composite-primary-keys/composite_primary_keys/blob/master/lib/composite_primary_keys/relation.rb
def add_cpk_support
      class << self
        include CompositePrimaryKeys::ActiveRecord::Batches
        include CompositePrimaryKeys::ActiveRecord::Calculations
        include CompositePrimaryKeys::ActiveRecord::FinderMethods
        include CompositePrimaryKeys::ActiveRecord::QueryMethods
https://github.com/strmpnk/composite_primary_keys/commit/21e081359eaa60a6b67bb1b999dbe4da23149f47

class CompositePrimaryKeys::ActiveRecord::Relation < ActiveRecord::Relation
  include CompositePrimaryKeys::ActiveRecord::Batches
  include CompositePrimaryKeys::ActiveRecord::Calculations
  include CompositePrimaryKeys::ActiveRecord::FinderMethods
  include CompositePrimaryKeys::ActiveRecord::QueryMethods

class ActiveRecord::Relation
  ...
  def self.new( klass, ... )
    klass.composite? ?
      CompositePrimaryKeys::ActiveRecord::Relation.new : self
  end

- Things that invalidate cache - https://github.com/charliesome/charlie.bz/blob/master/posts/things-that-clear-rubys-method-cache.

http://en.wikipedia.org/wiki/Inline_caching

- Partitioning
Cell architecture - Verify your partitinos??

How long now??

Down to 12ms! for the db.... 4seconds for the query...

Contacts selector
250ms

Serialization
Don't use activer record
Use postgres! - http://reefpoints.dockyard.com/2014/05/27/avoid-rails-when-generating-json-responses-with-postgresql.html

Ruby optimizations
Page caching
-
80% quote.

1B / 500 = 2M
2M / 44s = 88M seconds
88M seconds = 24444.4 hours or 2.8 years

@ 250ms, it took 5.8 days
