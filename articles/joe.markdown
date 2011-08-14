Title: Have Some Respect for the Uncaught Exception Handler
Author: Joe Hitchens
Date: Wed Aug 10 19:06:22 PDT 2011
Node: v0.4.10


	process.on("uncaughtException", function (err) {
		console.log("This shouldn't have happened (so we're taught)")
	});


We need to give the this event a little respect.
It really can be useful, if not crucial in some situations.


## Create the Image

The latest Node.js documentation (0.5.4 as of this writing) provides this
paragraph at the end of the description for the `uncaughtException` event.




[Sleepless Inc.]: http://www.sleepless.com/
