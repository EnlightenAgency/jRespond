#jRespond

####jRespond is a simple way to globally manage JavaScript on responsive websites.

Responsive websites that require JavaScript functionality for some breakpoints and not for others need some type of system for triggering the correct funcitons at the correct breakpoint and to also be aware of when a browser is resized across breakpoints. Although switching between breakpoints could be seen as an edge case, a few applications for jRespond are:

* Managing functionality for initial page load: Even if the browser is never resized, jRespond can help manage what JavaScript functions are triggered when the page loads for a given breakpoint.
* Development testing: jRespond makes it much easier to test in-browser.
* Borderline device widths: Real user browser resizing and device rotation that crosses breakpoints.

If your project only needs to support modern browsers I highly recommend checking out Rob Tarr's <a href="https://github.com/sparkbox/mediaCheck">mediaCheck</a> which uses the <code>matchMedia</code> method. But if you're using <a href="https://github.com/scottjehl/Respond">respond.js</a> as a polyfill to ensure that your site responds on older browsers, jRespond is worth checking out.

##How does it work?

jRespond is a script that holds a list of user-defined functions that are fired based on the browser's width compared to a list of customizable breakpoints. Entry and exit functions can be defined so transitions between breakpoints can be managed by removing and unbinding some page elements while creating and binding others. jRespond was built to be independent and browser agnostic. It does NOT sniff for media queries in the stylesheets.

After including jRespond.js, call jRespond and define as many or as few media breakpoints as you need for your project. Labels can be any single-word string:

``` JavaScript
// call jRespond and add breakpoints
var jRes = jRespond([
	{
		label: 'handheld',
		mq: 'only screen and (max-width: 767px)',
		enter: 0,
		exit: 767
	},{
		label: 'tablet',
		mq: 'only screen and (min-width: 768px) and (max-width: 979px)',
		enter: 768,
		exit: 979
	},{
		label: 'laptop',
		mq: 'only screen and (min-width: 980px) and (max-width: 1199px)',
		enter: 980,
		exit: 1199
	},{
		label: 'desktop',
		mq: 'only screen and (min-width: 1200px)',
		enter: 1200,
		exit: 10000
	}
]);
```

The mediaquery (mq) parameter is optional, and will fallback to use browser width if undefined.


Once running, functions can be registered with jRespond along with a desired breakpoint:

``` JavaScript
// register enter and exit functions for a single breakpoint
jRes.addFunc({
	breakpoint: 'desktop',
	enter: function(bp, prvBp) {
		myInitFunc();
	},
	exit: function(bp, prvBp) {
		myUnInitFunc();
	}
});
```

Or an array of breakpoints:

``` JavaScript
// register enter and exit functions for multiple breakpoints
jRes.addFunc({
	breakpoint: ['desktop','laptop'],
	enter: function(bp, prvBp) {
		myInitFunc();
	},
	exit: function(bp, prvBp) {
		myUnInitFunc();
	}
});
```

Use '*' to run a function at every breakpoint:

``` JavaScript
// register enter and exit functions for every breakpoint
jRes.addFunc({
	breakpoint: '*',
	enter: function(bp, prvBp) {
		myInitFunc();
	},
	exit: function(bp, prvBp) {
		myUnInitFunc();
	}
});
```

If you want to add a class to the body on each breakpoint change (with jQuery):

``` JavaScript
// register enter and exit functions for every breakpoint
jRes.addFunc({
	breakpoint: '*',
	enter: function(bp, prvBp) {
		$(document.body).addClass(bp);
	},
	exit: function(bp, prvBp) {
		$(document.body).removeClass(prvBp);
	}
});
```

Ask jRespond what the current or previous breakpoint is at any time:

``` JavaScript
// get the current breakpoint
jRes.getBreakpoint();
// get the previous breakpoint
jRes.getPrevBreakpoint();
```

The breakpoint parameter is required but the enter and exit parameters are optional (of course, at least one is required for something to happen).

The enter and/or exit functions are passed the current breakpoint and (if it exists) the previous breakpoint as parameters.

##Performance

jRespond is 1.3kb minified and only polls for the browser width every 500ms. If it detects a change the polling speed is increased to 100ms only until the browser width stops changing.

##Browser Support

IE 6+, Safari 5+, Firefox 3+, Chrome 1+

##Dependencies

None.

##Credits

Thanks to <a href="http://seesparkbox.com/foundry/author/rob_tarr">Rob Tarr</a> for inspiring the function registration pattern and <a href="http://markupboy.com/">Markup Boy</a> for helping me with my JavaScript failings.