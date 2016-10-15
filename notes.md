A Web Game From Scratch For A Game Jam
======================================

## Why a web game?

Because it runs everywhere.
Because you want your game to be played.

A web game does not require installation.
Simply type my cool game dot com and here we go.
On anything with a web browser.

## What is a Game Jam?

A coding competition with constraints. Time or size.

Some popular Game Jams are:

* [Ludum Dare](http://ludumdare.com)
* [js13kGames](http://js13kgames.com)
* [0h Game Jam](http://0hgame.eu)
* [One Game A Month](http://onegameamonth.com)

## The Theme

Usually a Game Jam gives a theme to make sure everybody is doing the game
for this very competition. Any game should match the theme in some way.

A theme is also helpful to give direction.

## Are there HTML5 game frameworks?

Plenty! The cool kids use:

* [Phaser](http://phaser.io)
* [PixiJs](http://pixijs.com)

Phaser is on the verge of becoming the jQuery of HTML5 gaming.

Actually, there are so many HTML5 engines, there's:

* [html5gameengine.com](http://html5gameengine.com)

## But we'll keep it simple

You simply don't need a framework to make a simple game.

And it's better to start bottom up to understand what really matters
and what does not.

Also, when size is a constraint, we may not want to include a framework
of library because of its size.

## How to start?

### First, make a decision what game you want to make.

Don't laugh, usually that's quite hard. Been there, done that.
You'll have plenty ideas. Each one just not good enough.
Or maybe they're equally good. Tough choices.

Making the decision early is vital. If you hesitate too long, there won't
be enough time to make it.

Speaking of Ludum Dare, for example, you should have decided on what game you
want to make by saturday noon. That's roughly one quarter in and if you
hesitate beyond this point, you most likely will run out of time.

### Make a prototype.

Try to develop in incremental steps.
Go from one working prototype to the next.
Always try to have a "playable" state.

Actually seeing your idea work is greatly inspiring.

### Then make graphics, music and levels.

Order is not important. But if your game has levels, I would start with that.
It's better to have an ugly game you can play than having a beautiful one you
can't play. Trust me.

## Pixel art? Vector graphics? 3D?

Depends on the type of the game. And your art skills. And your liking.

But generally, pixel art is a good way to start. It's quite easy to get
mediocre results, simply because there are just a few pixels to set.

Which doesn't mean pixel art is easy. There are some really impressive
artists out there. Great inspiration.

## Upsampling versus Pixel Art

Why is upscaling important? Well, in order to support as many screens
as possible, it's best to span the canvas over the full viewport.

Working with a fixed size canvas inside a bigger document doesn't work
very well on mobile devices for obvious reasons.

In order to render on a canvas with undefined size and proportions,
you'll want to scale your assets up or down depending on the size of
the canvas.

If you're making a platformer, the player entity should always have
the same relative size to the height of the canvas for example.

But traditionally, upscaling images in a browser gives blurry results
unfortunately. This is very unwanted for pixel art, of course.

Until recently, the only way to scale an image up and have crisp edges was
to manually rebuild the bigger image from the smaller one. Pixel by pixel.
It's not as bad as it sounds because you'd do that only once; before your
game starts. And it can also be a possibility to fine tune those pixels.

Now there's a CSS property to tell the browser you want to scale
pixelated:

	image-rendering: pixelated;

Alternatively you may use the imageSmoothingEnabled property of
CanvasRenderingContext2D to do the same for a given context:

	context.imageSmoothingEnabled = false;

## High DPI

High DPI displays are everywhere. Even on desktops. Prepare for that.

## Mobile

Most computers are cellphones today.

As Larry Page put it:

> We no longer live in a mobile first world,
> we live in a mobile only world.

Good thing is, it's quite easy to support mobiles too.
Canvas performance is quite good.

Although it varies over time. Not long ago, a Google Chrome update
totally broke Canvas rendering for some mobile devices.
Fortunately, that got fixed only a couple of month later.
You see the problem.

Still, I think the web is the best option when it comes to range.

## SVG scales great

SVG graphics scale great. But you can't draw SVG images on a Canvas.
Well, not easily that is. More on that later.

## gl.NEAREST

Scaling is at the very heart of WebGL.
So no problems with blurred images there.
Just use gl.NEAREST for Pixel Art:

	gl.texParameteri(
		gl.TEXTURE_2D,
		gl.TEXTURE_MIN_FILTER,
		gl.NEAREST)
	gl.texParameteri(
		gl.TEXTURE_2D,
		gl.TEXTURE_MAG_FILTER,
		gl.NEAREST)

But WebGL requires some background.

## Canvas, DOM, SVG oder WebGL?

WebGL is fastest:

* [Keep Out!](http://playkeepout.com)
* [HexGL](http://hexgl.bkcore.com/play)
* [BMW i8](http://playcanv.as/p/RqJJ9oU9?overlay=false)

If you want to render a lot of elements, there's no alternative.
And WebGL is supported by default now.
But that doesn't mean it runs on all devices, of course.

It's modern graphics programming. You have shaders and you translate,
scale and rotate by matrix math. Requires some background on the topic.

## Canvas is okay

* [Biolab](http://playbiolab.com)
* [Galex](http://galex.markusfisch.de)
* [Gingerman](http://gingerman.markusfisch.de)

On the desktop, performance varies greatly.
But if you don't have too many elements, it can be okay.
Support is much better than WebGL and you can reach even older
smartphones, computers and maybe even your TV or fridge.

It's classical graphics programming. 0/0 is in the left/top corner,
you can have alpha blending and drawing primitives is slow, of course.

## DOM only for a few elements

* [Striker](http://striker.markusfisch.de)
* [Santa Tracker](http://santatracker.google.com)

Can be okay for games with very few elements.
Scaling won't be that much of a problem.
And you can use jQuery (or whatever framework you're used to)
and every image format browsers know.

## What about SVG?

Since it's always slower to draw shapes than to copy memory (images),
don't expect too much. But if you have relatively few, very dynamic elements,
it can be an option. Here are some interesting examples:

* [Simple SVG Game](http://david.blob.core.windows.net/html5graphics/010_SimpleGame_SVGVersion.htm) ([Canvas implementation](http://david.blob.core.windows.net/html5graphics/009_SimpleGame_CanvasVersion.htm))
* [Animation Benchmark](http://themaninblue.com/experiment/AnimationBenchmark/svg/?particles=1000) ([Canvas implementation](http://themaninblue.com/experiment/AnimationBenchmark/canvas/?particles=1000))

## Anatomy of a 2D Canvas Game

Now that we have an overview, we want to have a look on the anatomy of
a simple 2D Canvas game.

We're going to use Canvas because it's very often the best choice between
support and performance. But that always depends on the game you want
to make. Especially on how many moving elements that game requires.

### HTML

	<!doctype html>
	<html>
	<head>
	<meta name="viewport" content="width=device-width, user-scalable=0"/>
	<style>
	...
	</style>
	</head>
	<body>
	<canvas id="Game">Sorry, no game for you.</canvas>
	<script>
	...
	</script>
	</body>
	</html>

### Stylesheet

	html, body {
		margin: 0; padding: 0;
		overflow: hidden;
		-ms-touch-action: none;
	}

	canvas {
		position: fixed;
		width: 100%;
		height: 100%;
	}

### JavaScript

	'use strict'

	var canvas,
		ctx,
		width,
		height,
		now,
		factor,
		last,
		pointersLength = 0,
		pointersX = [],
		pointersY = [],
		keysDown = []

	function draw() {}
	function run() {}
	function init() {}

### Init

	function init() {
		if (!(canvas = document.getElementById('Game')) ||
				!(ctx = canvas.getContext('2d'))) {
			return
		}

		window.onresize = resize
		resize()

		...

		last = Date.now() - 16
		run()
	}

### Resize

	function resize() {
		canvas.width = width = window.innerWidth
		canvas.height = height = window.innerHeight

		...
	}

### The main loop

	function run() {
		requestAnimationFrame(run)

		now = Date.now()
		factor = (last - now) / 16
		last = now

		input()
		draw()
	}

### Input

	function input() {
		if (pointersLength > 0) {
			move(pointersX[0] > playerX ? step : -step)
		} else if (keysDown[37]) {
			move(-step)
		} else if (keysDown[39]) {
			move(step)
		}
	}

### Draw

	function draw() {
		ctx.fillRect(0, 0, width, height)

		...
	}

### Touch and Mouse Input

	var D = document

	D.onmousedown = pointerDown
	D.onmousemove = pointerMove
	D.onmouseup = pointerUp
	D.onmouseout = pointerUp

	if ('ontouchstart' in D) {
		D.ontouchstart = pointerDown
		D.ontouchmove = pointerMove
		D.ontouchend = pointerUp
		D.ontouchleave = pointerUp
		D.ontouchcancel = pointerUp
	}

### Pointer down/move/up

	function pointerUp(event) {
		setPointer(event, false)
	}

	function pointerMove(event) {
		setPointer(event, pointerLength)
	}

	function pointerDown(event) {
		setPointer(event, true)
	}

### Set pointers

	function setPointer(event, down) {
		if (!down) {
			pointersLength = event.touches ?
				event.touches.length :
				0
		} else if (event.touches) {
			var touches = event.touches

			pointersLength = touches.length

			for (var i = pointersLength; n--;) {
				var t = touches[n];

				pointersX[i] = t.pageX
				pointersY[i] = t.pageY
			}
		} else {
			pointersLength = 1
			pointersX[0] = event.pageX
			pointersY[0] = event.pageY
		}

		event.preventDefault()
	}

### Keyboard Input

	var D = document

	D.onkeydown = keyDown
	D.onkeyup = keyUp

### Set pressed keys

	function setKey(event, down) {
		keysDown[event.keyCode] = down
		event.preventDefault()
	}

	function keyUp(event) {
		setKey(event, false)
	}

	function keyDown(event) {
		setKey(event, true)
	}

## Demo

That was a lot of boring code.
Now have a look at all that in action:

* [Canvas](http://markusfisch.github.io/web-game-from-scratch-talk/canvas.html)
* [WebGL 2D](http://markusfisch.github.io/web-game-from-scratch-talk/webgl.html)

View the source.

## Tips

### Keep it simple!

IIFE ([Immediately-Invoked Function Expression](http://benalman.com/news/2010/11/immediately-invoked-function-expression))
doesn't make much sense on a single page game.

Don't overcomplicate things. Favor pure functions over OO.
OO usually means a lot of allocations and lookups.
For a small, simple game, it's just not worth the overhead.

As everybody knows, keeping things simple is the key to good software
and a time constraint competition is not an exception.

Start as simple as possible.

### All numbers are floats!

When drawing on a Canvas, always make sure to use integers:

	ctx.drawImage(sprite, x | 0, y | 0)

Canvas will try to interpolate an image if you give floats which results
in blurry images. Have a look at [HTML5 canvas optimisation](http://seb.ly/2011/02/html5-canvas-sprite-optimisation).

### Object Properties and Array Indicies are slow

Resolving a property or an index takes time. So avoid this:

	object.a.b.c()

	array[0][0][0]

And better do that:

	var a = object.a,
		b = a.b

	b.c()

Lookup's take time. a[0][0] are two lookups. a[0] is just one.

Check out [JavaScript performance tips](http://moduscreate.com/javascript-performance-tips-tricks) for more info on the topic.

### Avoid allocations

Managing memory takes time. Especially doing garbage collection is not trival
and if you allocate a lot of objects the garbage collecter has to free, you're
slowing things down.

So try to avoid any of that in the hot path:

	object = []

	object = {}

	new Object()

### Prerender

Draw frequently used elements in a Canvas element and draw this with

	ctx.drawImage(sprite, x | 0, y | 0)

Working with (rastered) images is always the fastest way to draw.

### Don't scale or rotate in the hot path

Scaling and rotating are expensive operations in Canvas.
It's better to prerender those transformations and simply use those images.

### Schedule onresize events

	function scheduleResize() {
		if (resizeTimer) {
			clearTimeout(resizeTimer)
		}

		resizeTimer = setTimeout(resize, 200)
	}

	document.onresize = scheduleResize

### Run in Full Screen Mode on Mobile Devices

If you want your game to run in full-screen mode on mobile devices,
you need to add meta tags for iOS and manifest files for Chrome and
Firefox on Android.

### Meta Tags on iOS

For iOS, and this works only if the game is started from the home screen
(after it was added there):

	<meta name="apple-mobile-web-app-capable" content="yes"/>
	<meta name="apple-mobile-web-app-status-bar-style" content="black"/>

### manifest.webapp for Chrome on Android

	{
		"name": "...",
		"installs_allowed_from": ["*"],
		"fullscreen": "true"
		...
	}

### manifest.json for Firefox

	{
		"start_url": "index.html",
		"display": "standalone"
		...
	}

### Texture atlas

Use a texture atlas, also known as a sprite sheet, to manage game assets.
It's smaller, faster and chances are you want to resize the assets anyway
because

	ctx.drawImage(
		src, sx, sy, sw, sh,
		dst, dx, dy, dw, dh)

is a perfect fit.

Also, when you intend to work with WebGL some day, it's probably a good
idea to get used to the idea of a texture atlas.

### devicePixelRatio / backingStorePixelRatio

Canvas on a High DPI display does have some properties to get the pixel
ratio between "web" pixels and true pixels:

	ratio =
		(window.devicePixelRatio || 1) /
		(ctx.webkitBackingStorePixelRatio ||
			ctx.mozBackingStorePixelRatio ||
			ctx.msBackingStorePixelRatio ||
			ctx.oBackingStorePixelRatio ||
			ctx.backingStorePixelRatio ||
			1)

With that, you can multiply the dimensions and locations:

	width = window.innerWidth * ratio | 0
	height = window.innerHeight * ratio | 0

	for (var i = 0; i < pointerLength; ++i) {
		pointersX[i] = pointersX[i] * ratio | 0
		pointersY[i] = pointersY[i] * ratio | 0
	}

Read [here](http://phoboslab.org/log/2012/09/drawing-pixels-is-hard) why
there is devicePixelRatio and backingStorePixelRatio.

### ctx.fillRect() versus ctx.clearRect()

The background of a transparent 2D context is undefined.
Undefined does not mean black.

So when we are using a 2D context with alpha set to false

	ctx.getContext("2d", {alpha: false})

to speed up rendering (in theory), it's important to fill the
canvas explicitly because we never know what background color
the browser sets. Chrome used to draw black. Now it's white
(at least on some platforms).

### Use two canvas on top of each other

To speed up drawing, you may draw the immovable background in a background
Canvas element and the action in a Canvas element on top of it.

Since browsers are very good at composing elements, this can be quite
performant.

### Avoid State-Changes

Canvas is a state machine so state changes hurt performance.

	// avoid state changes
	ctx.save()
	...
	ctx.restore()

	// group by path
	ctx.beginPath()
	...
	ctx.closePath()

	// group by fillStyle
	ctx.fillStyle = “red”;

Read more on [Canvas Performance](http://www.html5rocks.com/en/tutorials/canvas/performance).

## WebAudio

Use the WebAudio API to play sound. Very simple.

* [Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API)
* [Using Web Audio](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API/Using_Web_Audio_API)

## Profile

Don't pre-optimize. Test, profile, then optimize.

## Just do it!

It's really great big fun to write your own game!

## Ressources

* [Getting started with HTML5 game development](https://hacks.mozilla.org/2013/09/getting-started-with-html5-game-development)
* [Guides for Game Development](https://developer.mozilla.org/en-US/docs/Games)
* [Optimizing HTML5 Canvas Games](http://kodogames.com/optimising-html5-canvas-games)
* [Optimizing for Firefox OS](https://hacks.mozilla.org/2013/05/optimizing-your-javascript-game-for-firefox-os)
* [Canvas Cheat Sheet](https://simon.html5.org/dump/html5-canvas-cheat-sheet.html)
* [HTML5 Game Devs](http://www.html5gamedevs.com)
* [SVG guide](http://www.sitepoint.com/the-complete-guide-to-building-html5-games-with-canvas-and-svg)
* [Guide to HTML5 Canvas Libraries](https://ihatetomatoes.net/guide-to-html5-canvas-javascript-libraries/)

## Questions?

## Thanks!
