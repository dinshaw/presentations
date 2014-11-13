## i :heart: Promises in Ruby
Promises we can keep.

"I promise that there will be no cat pics."

Dinshaw Gobhai | [dgobhai@constantcontact.com](mailto:dgobhai@constantcontact.com)

@tallfriend

[github.com/dinshaw/promises-in-ruby](github.com/dinshaw/promises-in-ruby)

[github.com/dinshaw/promises](github.com/dinshaw/promises)

***

## Promises, eh?
"direct correspondence between synchronous and asynchronous functions"

[Callbacks, Promises, and Coroutines](https://www.youtube.com/watch?v=V2Q13hzTGmA) - Domenic Denicola

[You’re missing the point of Promises](https://blog.domenic.me/youre-missing-the-point-of-promises/) - Domenic Denicola

[Promises/A+](https://promisesaplus.com/) - Lots of very smart people
***

## Promises
<p class='fragment'>* Return a promise, immediately</p>
<p class='fragment'>* Executes</p>
<p class='fragment'>* Fulfills or Rejects</p>
<p class='fragment'>* Chainable / "thenable"</p>

***

## Fulfill and Reject

```ruby
    Promise.new { |fulfill, reject|
      if something
        fulfill.call something.value
      else
        reject.call something.error
      end
    }
```

***

## Thanks!
Brian Mitchel and Mike Davis for explaining this stuff to me.
Jed Northbridge for his Reavel-CK gem that I used to make this presentation

***

## Thoughts
* EM

