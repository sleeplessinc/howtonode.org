Title: Have Some Respect for the Uncaught Exception Handler
Author: Joe Hitchens
Date: Wed Aug 10 19:06:22 PDT 2011
Node: v0.4.10


	process.on("uncaughtException", function (err) {
		console.log("This shouldn't have happened (so we're taught)")
	});


We need to give the this event a little respect.
It can be very useful, if not crucial in some situations.

The latest Node.js documentation (0.5.4 as of this writing) provides this
paragraph at the end of the description for the `uncaughtException` event:

> Note that uncaughtException is a very crude mechanism for exception handling. Using try / catch in your program will give you more control over your program's flow. Especially for server programs that are designed to stay running forever, uncaughtException can be a useful safety mechanism.

I suspect that the first sentence in this paragraph is the source of
some stigma for the poor event.
However, this article is about the second sentence.

If an Exception is thrown and is not otherwise caught, it will rise all the way up
to the underying event loop.  When that happens, the currently defined
`uncaughtException` handler is called if it has been set up.
If one hasn't been put in place, then a stack trace will be printed and the process
will exit with return code 1.

In a perfect world, every exception would be intentionally caught and dealt with 
in a graceful fashion. Given the current availability of perfect worlds, let's 
talk about imperfect worlds, like the real world.

In the real world, servers typically must be designed for robustness.
They need to provide 100% uptime.
My long experience has shown that it's extraordinarily difficult to build 
a software train that will stay on the track you've laid 100% of the time
and in 100% of the possible situations that may arise.  
I am not saying it can't be done, just that it's very difficult and therefore,
very costly.

Why is this?

Most of the work that my company does
([Sleepless Software Inc.](http://sleepless.com/))
revolves around 1 or more (often all) of the following scenarios:

* Cutting-edge technology
* Short time-frames
* High pressure
* Minimal budget
* High tech startups

This usually means that we rely on very skilled and experienced developers
who know how to git [sic] down in the trench and engage in some hard-core
guerilla coding in order to get things done quick.  We build things that work.
Graceful is great.  Simple is better.  And whatever gets the job done is always
an option.  I will claim that anyone who has been in software developement
for any length of time will know what I'm talking about.  Code is never perfect,
and there's almost always a tipping point in terms of deadlines where you
are forced to put away the little chisel and reach for the chainsaw, and 
everyone starts murmuring things like,
"... and then we'll come back and do it right later".

A recent project was a Facebook game called "Family Village".
Though only recently released, it was designed for very large scale running
on AWS and with a game server entirely written in Javascript.
This custom server uses Node.js and is very efficient and robust.
Part of the robustness derives from a concious decision to RELY on 
the `uncaughtException` handler to catch and effectively 
deal with the unexpected. 

The game server uses RESTful HTTP transactions to communicate with the game client.
Also, although it may not be long before it is possible, one can't currently
throw an Exception that will cross the asynchronous callbacks that Node.js
depends so heavily on.  Because of these factors, and the real-world,
we found that as traffic increased, new corner-case errors, and net weather 
would often result in HTTP transactions that weren't closed out properly.

The following technique deals with this in a relatively simple way.
The benefit it provides is a way of catching and cleaning up 
transactions that fail to be gracefully closed out, regardless of why they 
failed.
The idea is that there will always be rare occasions when an unforeseen error 
or corner-case arises that made it past testing into production.
In those rare cases, the `uncaughtException` handler would be called.
It can then attempt to log and/or send a message to someone so that
the exception can be tracked down, analyzed, and resolved while the server
carries on handling traffic.


In the real world, projects often rely on third-party software modules, multiple
developers with varying degrees of skill and experience, testing departments that
may vary from excellent to non-existent, etc.



[Sleepless Inc.]: http://www.sleepless.com/


