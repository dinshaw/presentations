## Intro
Meet the SLA's what does SLA mean?
In hopes of making this talk accesible to eeryone, I am going to try and ekplain things that may be obvious to some. if you're not a novice, feel free to correct me!
one of the reasons i'm catering to a novice crowd is because that is what we are working with as we build rails in large teams. Why do I say this? There is no more mature talent on the market. We have a very active apprentice program. When we started buildinvg this app, we were faced with the Unique problem of app having to work at a scale that most startups don't hit for the lifetime of the business, as soon as we throw the switch.
I'm going to walk through our two-year road from design to full adoption.
TL;DR :
Compostite primary keys are amazing
Don't ues AR, if you're never going to need it
Careful with loops
Pull requests
Code reviews
KISS

But this is more than a story of Rails at scale, its also a story of accuisition and cultural assimilation.
What's it like going from a lean start-up to a huge company with a ton of existing infrustructure, processes, and legacy systems that are still verymuch in use and serving 100s of 1000s of people

Large team issues:
** Not everyone on your team is going to be a rockstar
** tech lead will miss something

## Start
Contact management
in office with bullet hole saying "ok, i guess we'll need an index page".
none of us had any experience building to scale.
We were starting from scratch to replace something that had zero documentation and 500k users.
The UI resembled the offspring of Frankenstine and an excel sheet.
I'm saying this for a couple reasons, one, its funny, two : to illustrate the lack of respect we had for the existing app when we started.
Programmers have a tendancy to shit on everything and pronounce it needs to be re-written, possibly in the language of the month. As our standands galvanize, critiques become less and less subjective. Start-up acquisition comes with a lot of emotional bagage. You think you're better than them, you don't respect their old ways and this lack of respect can cost you time, because you don't take advantage of all the knowledge that they have amassed. Why, because you feel you can do it better, and because its hard to get to.

So we started imagining what we wanted to bring to life. We started looking at front-end frameworks
* Sammy - Did not give us as much out of the box at that time.
* Backbone - what version? terible docs

Started working on the domain.

We'll build two apps, one api and one client. We will consume our own api so it will be awesome!
Get started docs... start up the app, then start up the other app.
OK

Started writting tests, like we do now to run the tests you have to start up both apps.
At somepoint we realized that onboarding was becoming a problem.

So we consolodated the apps. Huge win.
Rails forms, etc
THen we moved from sinatra and artifice to webmock. Huge win number two.
Everything started feeling like rails dev again

Started mapping out a contact, that had many service-addresses, and addresses, and company data until we were like wait a min, this looks like a document.
So we decided couch would be a perfect fit. We then spent the next three months trying to convince the company that this was a good idea.
THey agreed, but told us it wasn't going to happen.
This was a bit of a blow to us four freelancers. WHy wouldn't this 10yr old company with 1500 employees and 1500 servers listen to us.
This was the first point where we were beginning to become aware that we were part of something much bigger than our app.
The infrustructure around a end-to-end workflow is staggering. And although we do have dedicated ops, x people to x servers means that things have to stay in control.

We had resident mysql expertise, so the descussion the was basically around how to set up mysql. Postgres was mentioned, but again, no experience with it.
DB2 was suggested but the idea was to get off of it.
So we are like 'Postgres is great, we'll run it. We don't need you.' Thank god they didn't listen to us.
Over the next few months we decided on a cell architecture which looks a little like this :

- Explain cell

And then it was time to check in with our SLAs.
40 seconds for what was supposed to be a 500 ms call.
I said, no worries, I expected that. We were writing decent code, but we were also moving fast, so as long as it was maintainable, and easy to read, we checked it in.
So who goes to look at performance.
Guess who? All the java guys whom we had dragged up to speed on ruby.
Suddenly, there were benchmark blocks around every reource intensive line of code in the app.
There were load tests being run against load-balanced local clusters.
THere were two different profilers runninng dtrace and ruby-profiler.
And a the dbas were analyizing the querys.

This was the first ahha momoent for me where I was greatful for the machine.
All around me things were getting done that I didn't really know how to do.
Sadly, I had to keep writing code, so i was not able to just watch and learn, but thankfully, when the dust setteled, i got to prepare this talk, durring which i figuered out what had happened.

So low-hanging fruit first:
- WE cleaned up the ruby

