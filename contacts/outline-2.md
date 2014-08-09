
    some of the bullets are descriptive - setting the scene - others have value and are the talking points

# Contacts Outline

## Common problems
*- Replace a huge undocumented app with paying users
*+

## Unique problem
*- Building an app for immediate scale
*+

## Getting Started
*- Single page app - JS Framework shoot-out
*+ I'd go with Turbolinks now - why? Overengieering - my interview

*- Started Index page, and the Domain

*- Two Apps - our own consumer
*- Getting started docs; np!
*- Add solr, add stubs,
*- Getting started docs now include launchctl
*+ terrible idea; we would later consolidate - then heard some similar stories

*+ Consolidated apps
*+ Switched to webmock

*- Document store would be great, shot down by ops
*+ So glad we didn't go there - knowing the hurdles we had to overcome with familiar technolegies, god help us if we had to use new ones.

*** Keep it simple; if it seems so easy that you could do it in a day, then you are on the right track. If you have never done anything this size, this x, this y, then you have never done anything like this. Pessimisam is the mother of success
We had MySQL experts on staff. We went with it.

## Cell architecture

Check in with SLA
*- 500 get ran at 44 seconds. SLA was 500ms
*- I was expecting this. Rapid dev, etc
*+ Guess who stated looking at it? All the peoples i had written off as out of date.

We made it after about 3 months of investigation and work

Before any metrics were in, First step was to get the db right.
*+ Moved to a composite key query - Why is this good? Faster queries? better than an indexed foreign key?
*+ DB Partitioning
*- issue with not enough partitions
* After db partitioning, we had the get down to 12ms, but the whole call was still taking 4s
* Enter JAVA Contacts query tool. We realized that there was no need for going to AR. in retrospect, this tool prob could have been pure ruby, but that is not where we had resources. down to 385ms
http://reefpoints.dockyard.com/2014/05/27/avoid-rails-when-generating-json-responses-with-postgresql.html

* Load and performance servers were exposing the deficiencies at a high level.
** Ruby profiler was helpful for seeing time spent in various levels, but we needed to see trends

    Benchmark do
      ...
    end

    Now this can be done with NR
* Ruby loops
* ** Patched CPK gem

    https://github.com/strmpnk/composite_primary_keys/commit/21e081359eaa60a6b67bb1b999dbe4da23149f47

- Megamorphic problem

    class Foo
      def f
      end
    end

    class Bar
      def f
      end
    end

    def hot_code_path(x)
      x.f # inline cache (polymorphic), eventually JITs
      case x
      when Foo
      when Foo + <<unique class>>
      else
        # long style lookup
      end
    end

    a = [Foo.new, Bar.new] * 1000

    a.each {|o|
      def o.other; end
      hot_code_path(o) #  inline cached, eventually JITs
    }

- Jruby hit compiling worse than ruby because its more aggressive
- Explain singleton issue
- inline cache
- trace surfaced more v2 slowness


Connection pools: TB was not returning connections in v2.x, needed to wrap in connection block.


