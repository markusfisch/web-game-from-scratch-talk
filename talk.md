A Web Game From Scratch For A Game Jam
======================================

## Why a web game?

Because it runs everywhere.
Because you want your game to be played.

A web game means no installation.
Simply type http my cool game dot com and play.

## What is a Game Jam?

A short coding competition.

Some popular samples are:

* [Ludum Dare](http://ludumdare.com)
* [js13kgames](http://js13kgames.com)
* [0h Game Jam](http://0hgame.eu)
* [One Game A Month](http://onegameamonth.com)

## The theme

Game Jams very often give a theme to make sure everybody is doing the game
for this very competition. So your game should match the theme in some way.

A theme is also helpful to give a Jam direction.

## Are there HTML5 game frameworks?

Plenty! The cool kids use:

* [Phaser](http://phaser.io)
* [PixiJs](http://pixijs.com)

Phaser is on the verge to become the jQuery of HTML5 gaming.

In fact, there are so many engines, there's a site for them:

* [html5gameengine.com](http://html5gameengine.com)

## But we'll keep it simple

You simply don't need a framework to make a simple game.

And it's better to start bottom up to understand what really matters
and what does not.

## How to start?

### First, make a decision what game you want to make.

Don't laugh, usually that's quite hard. Been there, done that.
You'll have plenty ideas, each one just not good enough.
Or maybe equally good.

Making the decision early is vital. If you hesitate too long, there won't
be enough time to make it.

Speaking of Ludum Dare, for example, you should have decided on what game you
want to do by saturday noon. That's roughly one quarter in and if you hesitate
beyond this point, there simply won't be enough time.

### Make a prototype.

Try to make incremental steps. Go from one working prototype to the next.

### Then make graphics, music and levels.

Order is not important. But if your game has levels, I would start with that.
It's better to have an ugly game you can play than having a beautiful one you
can't play. Trust me. I can give examples.

## Pixel art? Vector graphics? 3D?

Depends on the type of the game. And your art skills. And your liking.

But generally, pixel art is a good way to start. It's quite easy to get
mediocre results, simply because there are just a few pixels to set.

Which doesn't mean pixel art is easy. There are some really impressive
artists out there.

## Upsampling versus Pixel Art

Traditionally, scaling images up in a browser makes them blurry.
That is, of course, unwanted for pixel art.

Until recently, the only way to scale an image up and have crisp edges was
to manually rebuild the bigger image from the smaller one. Pixel by pixel.
It's not that bad as it sounds because you'd do that only once, when your
game starts.

Now, there's a new CSS property you can use for that:

image-rendering: pixelated;

For some background vist [Drawing pixels is hard](http://phoboslab.org/log/2012/09/drawing-pixels-is-hard).

## High DPI

High DPI displays are everywhere. Even on desktops. Prepare for that.

## Mobile

Most computers are cellphones today.

As Larry Page put it:

> We no longer live in a mobile first world,
> we live in a mobile only world.

Good thing is, it's quite easy to support mobiles too.
Canvas performance is quite good.

Although - it varies over time.

Not long ago, a Google Chrome update totally broke Canvas rendering for some
mobile devices. Fortunately, that got fixed a couple of month later.
But you see the problem.

Still, I believe, the web is the best bet when it comes to range.

## SVG scales great

SVG graphics scale great. But you can't draw SVG images on a Canvas.
Well, not easily that is. More on that later.

## gl.NEAREST

Scaling is at the very heart of WebGL.
So no problems with blurred images there.

## Canvas, DOM, SVG oder WebGL?

WebGL is fastest:

* [Demo](http://hhsw.de/sites/proto/webgl)
* [Play Keep out](http://playkeepout.com)
* [HexGL](http://hexgl.bkcore.com/play)
* [BMW i8](http://playcanv.as/p/RqJJ9oU9?overlay=false)

If you want to render many elements, there's no alternative.
And WebGL is supported by default now.
But it doesn't run on all devices, of course.

It's modern graphics programming. You have GLSL shaders and you translate,
scale and rotate by matrix math. Requires some background on the topic.

## Canvas is okay

* [Biolab](http://playbiolab.com)
* [Galex](http://galex.markusfisch.de)
* [Gingerman](http://gingerman.markusfisch.de)

On the desktop, performance varys greatly.
But if you don't have too many elements, it can be okay.
Support is much better than with WebGL and you can reach even older
smartphones and computers.

It's classical graphic programming. 0/0 is left/top, you can have alpha
blending but drawing primitives is slow, of course.

## DOM only for a few elements

* [Striker](http://striker.markusfisch.de)
* [Santa Tracker](http://santatracker.google.com)

Can be okay for games with very few elements.
Scaling won't be a problem.
You can use jQuery and every image format browsers know.

## What about SVG?

Well, that's largely uncharted territory.
Since it's always slower to draw shapes than to copy memory, don't
expect too much. But if you have realtively few, very dynamic elements
it can be an option.

* [Simple SVG Game](http://david.blob.core.windows.net/html5graphics/010_SimpleGame_SVGVersion.htm) ([Canvas implementation](http://david.blob.core.windows.net/html5graphics/009_SimpleGame_CanvasVersion.htm))
* [Animation Benchmark](http://themaninblue.com/experiment/AnimationBenchmark/svg/?particles=1000) ([Canvas implementation](http://themaninblue.com/experiment/AnimationBenchmark/canvas/?particles=1000))
* [SVG guide](http://www.sitepoint.com/the-complete-guide-to-building-html5-games-with-canvas-and-svg)

## Anatomy of a 2D Canvas Game

Now that we have an overview, we want to have a look on the anatomy of
a simple 2D Canvas game.

### HTML

	<!doctype html>
	<html>
	<head>
	<meta name="viewport" content="width=device-width"/>
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

	html, body
	{
		margin: 0; padding: 0;
		overflow: hidden;
		-ms-touch-action: none;
	}

	canvas
	{
		position: fixed;
		left: 0; right: 0;
		top: 0; bottom: 0;
	}

### JavaScript

	"use strict"

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

	function init()
	{
		if( !(canvas = document.getElementById( "Game" )) ||
			!(ctx = canvas.getContext( "2d" )) )
			return

		window.onresize = resize
		resize()

		...

		last = Date.now()-16
		run()
	}

### Resize

	function resize()
	{
		canvas.width = width = window.innerWidth
		canvas.height = height = window.innerHeight

		...
	}

### The main loop

	function run()
	{
		requestAnimationFrame( run )

		now = Date.now()
		factor = (last-now)/16
		last = now

		input()
		draw()
	}

### Input

	function input()
	{
		if( pointersLength > 0 )
			move( pointersX[0] > playerX ? step : -step )
		else if( keysDown[37] )
			move( -step )
		else if( keysDown[39] )
			move( step )
	}

### Draw

	function draw()
	{
		ctx.fillRect( 0, 0, width, height )

		...
	}

### Touch and Mouse Input

	{
		var D = document

		D.onmousedown = pointerDown
		D.onmousemove = pointerMove
		D.onmouseup = pointerUp
		D.onmouseout = pointerUp

		if( "ontouchstart" in D )
		{
			D.ontouchstart = pointerDown
			D.ontouchmove = pointerMove
			D.ontouchend = pointerUp
			D.ontouchleave = pointerUp
			D.ontouchcancel = pointerUp
		}
	}

### Pointer down/move/up

	function pointerUp( event )
	{
		setPointer( event, false )
	}

	function pointerMove( event )
	{
		setPointer( event, pointerLength )
	}

	function pointerDown( event )
	{
		setPointer( event, true )
	}

### Set pointer

	function setPointer( event, down )
	{
		if( !down )
		{
			pointersLength = event.touches ?
				event.touches.length :
				0
		}
		else if( event.touches )
		{
			var touches = event.touches

			pointersLength = touches.length

			for( var n = pointersLength; n--; )
			{
				var t = touches[n];

				pointersX[n] = t.pageX
				pointersY[n] = t.pageY
			}
		}
		else
		{
			pointersLength = 1
			pointersX[0] = event.pageX
			pointersY[0] = event.pageY
		}

		event.preventDefault()
	}

### Keyboard Input

	{
		var D = document

		D.onkeydown = keyDown
		D.onkeyup = keyUp
	}

### Set pressed keys

	function setKey( event, down )
	{
		keysDown[event.keyCode] = down
		event.preventDefault()
	}

	function keyUp( event )
	{
		setKey( event, false )
	}

	function keyDown( event )
	{
		setKey( event, true )
	}

## Tips

### Schedule onresize events

	function scheduleResize()
	{
		if( resizeTimer )
			clearTimeout( resizeTimer )

		resizeTimer = setTimeout( resize, 200 )
	}

	document.onresize = scheduleResize

### All numbers are floats!

When drawing on a Canvas, always make sure to use integers:

	ctx.drawImage( sprite, x | 0, y | 0 )

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
ctx.drawImage( sprite, x, y ).

Working with (rastered) images is always the fastest way to draw.

### Don't scale or rotate in the hot path

Scaling and rotating are expensive operations in Canvas.
It's better to prerender necessary assets and just draw those.

### Keep it simple!

IIFE ([Immediately-Invoked Function Expression](http://benalman.com/news/2010/11/immediately-invoked-function-expression))
doesn't make much sense on a single page game.

Don't overcomplicate things. Try not to use OO. For a small, simple game,
it's just not worth the overhead.

### Texture atlas

Use a texture atlas, also known as a sprite sheet, to manage game assets.
It's smaller, faster and chances are you want to resize the assets anyway
so ctx.drawImage( src, sx, sy, sw, sh, dst, dx, dy, dw, dh ) is a perfect
fit.

Also, when you intend to work with WebGL some day, it's probably a good
idea to get used to the idea of a texture atlas.

### devicePixelRatio / backingStorePixelRatio

Canvas on a High DPI display does have some properties to get the pixel
ratio between "web" pixels and true pixels:

	ratio =
		(window.devicePixelRatio || 1)/
		(ctx.webkitBackingStorePixelRatio ||
			ctx.mozBackingStorePixelRatio ||
			ctx.msBackingStorePixelRatio ||
			ctx.oBackingStorePixelRatio ||
			ctx.backingStorePixelRatio ||
			1)

With that, you can multiply the dimensions and locations:

	width = window.innerWidth*ratio | 0
	height = window.innerHeight*ratio | 0

	for( var i = 0; i < pointerLength; ++i )
	{
		pointersX[i] = pointersX[i]*ratio | 0
		pointersY[i] = pointersY[i]*ratio | 0
	}

The reason why there is devicePixelRatio and backingStorePixelRatio,
you may read [here](http://phoboslab.org/log/2012/09/drawing-pixels-is-hard).

### ctx.fillRect() versus ctx.clearRect()

	ctx.getContext( "2d", { alpha: false } )

The background of a 2D context is undefined. Undefined does not mean black.
So when we are using a 2D context with alpha set to false to speed up rendering,
it's important to fill the canvas explicitly.

### Use two canvas on top of each other

To speed up drawing, you may draw the immoveable background in a background
Canvas element and the action in a Canvas element on top of it.

Since browsers are very good at composing elements, this can be quite effective.

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

* [Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API)
* [Using Web Audio](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API/Using_Web_Audio_API)

## Profile

Don't pre-optimize. Test, then optimize.

## Just do it!

It's really fun to write your own little game. Try it!

## Ressources

* [Getting started with HTML5 game development](https://hacks.mozilla.org/2013/09/getting-started-with-html5-game-development)
* [Guides for Game Development](https://developer.mozilla.org/en-US/docs/Games)
* [Optimizing HTML5 Canvas Games](http://kodogames.com/optimising-html5-canvas-games)
* [Optimizing for Firefox OS](https://hacks.mozilla.org/2013/05/optimizing-your-javascript-game-for-firefox-os)
* [Canvas Cheat Sheet](https://simon.html5.org/dump/html5-canvas-cheat-sheet.html)
* [HTML5 Game Devs](http://www.html5gamedevs.com)

## Questions?

## Thanks!
