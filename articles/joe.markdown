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

In the real world, projects often rely on third-party software modules, multiple
developers with various degrees of skill, and operation parameters that require
100% uptime.



[Sleepless Inc.]: http://www.sleepless.com/


