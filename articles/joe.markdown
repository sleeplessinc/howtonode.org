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

	Note that uncaughtException is a very crude mechanism for exception handling. Using try / catch in your program will give you more control over your program's flow. Especially for server programs that are designed to stay running forever, uncaughtException can be a useful safety mechanism.


[Sleepless Inc.]: http://www.sleepless.com/


