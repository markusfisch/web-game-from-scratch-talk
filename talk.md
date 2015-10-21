A Web Game From Scratch
=======================

## Why in the browser?

Because it runs everywhere. Because you want your game to be played.
No installation, no hazzles. Simply http my cool game dot com and play.

## What is a Game Jam?

A short competition.

http://ludumdare.com

http://js13kgames.com

http://0hgame.eu

http://onegameamonth.com/

## The theme

Game Jams very often give a theme to make sure, everybody is doing the game
for this very competition.

## Are there HTML5 game frameworks?

Plenty! Some popular choices are:

http://phaser.io
http://pixijs.com

Phaser is becoming the jQuery of HTML5 gaming.

In fact, there are so many, there's a site for HTML5 game engines:

http://html5gameengine.com

## But we'll keep it simple

You don't need a framework. It's all quite simple. And fun.

## How to start?

### Make a decision what game you want to do.

Don't laugh, that's the hardest part. Been there, done that.
Usually, you'll have plenty ideas, each one just not good enough.
Or maybe equally good.

Speaking of Ludum Dare, for example, you should have decided on what game you
want to do by saturday noon. That's roughly one quarter in and if you hesitate
beyond this point, there will be not enough time.

### Make a prototype.

Try to make incremental steps. Go from one working prototype to the next.

### Then make graphics, music and levels.

Order is not important. But if your game has levels, I would start with that.
It's better to have an ugly game you can play than having a beautiful one you
can't play. Trust me.

## Pixel art? Vector graphics? 3D?

Depends on the type of the game. And your skills. And your liking.
But generally, pixel art is a good way to start. It's quite easy to get
mediocre results, simply because there are just a few pixels to set.
Which doesn't mean pixel art is simple.

## Upsampling versus Pixel Art

Traditionally, scaling images up in a browser makes them blurry.
That is, of course, very unwantet for pixel art.

Until recently, the only way to scale an image up and have crisp edges was
to manually rebuild the bigger image from the smaller one. It's not that
horrible as it sounds because you'd do that only once, when your game starts.

Now, there's a new CSS property you can use for that:

image-rendering: pixelated;

## High DPI

Retina displays are everywhere.

http://phoboslab.org/log/2012/09/drawing-pixels-is-hard

## Mobile

Most computers are cellphones today.

As Larry Page famously put it:
We no longer live in a mobile first world,
we live in a mobile only world.

Good thing is, it's quite easy to optimize for mobiles from the start.
Canvas performance is quite good on mobile. Although that varies over time.

Not long ago, an Google Chrome update totally broke Canvas rendering on some
mobile devices. Fortunately, that got fixed a couple of month later.

## SVG scales great

SVG graphics scale great. But you can't draw SVG images on a Canvas.
Well, not like raster files at least.

## gl.NEAREST

Scaling is at the very heart of WebGL. So no problem there.

## Canvas, DOM, SVG oder WebGL?

WebGL is fastest:

http://hhsw.de/sites/proto/webgl/

http://playkeepout.com

http://hexgl.bkcore.com/play

If you want to render many elements, there's no alternative.
And WebGL is supported by default now.
But it doesn't run on all devices, of course.

It's modern graphics programming. You have GLSL shaders and translate,
scale and rotate by matrix math. It requires some background.

## Canvas is okay

http://playbiolab.com/

http://galex.markusfisch.de

http://gingerman.markusfisch.de

On the desktop, performance varys greatly.
But if you don't have too many elements, it can be quite okay.

It's classical graphic programming. 0/0 is left/top, it supports alpha
blending but drawing primitives is slow, of course.

## DOM only for a few elements

http://striker.markusfisch.de

http://santatracker.google.com

Can be okay for games with very few elements.
Scaling won't be a problem.
You can use jQuery and every image format browsers know.

Google did the Santa Tracker 2014 in DOM.

## What about SVG?

Well, that's largely uncharted territory but here are some samples:

http://david.blob.core.windows.net/html5graphics/010_SimpleGame_SVGVersion.htm

http://david.blob.core.windows.net/html5graphics/009_SimpleGame_CanvasVersion.htm

http://themaninblue.com/experiment/AnimationBenchmark/svg/?particles=1000

http://themaninblue.com/experiment/AnimationBenchmark/canvas/?particles=1000

http://www.sitepoint.com/the-complete-guide-to-building-html5-games-with-canvas-and-svg/

https://msdn.microsoft.com/en-us/library/gg589521(v=VS.85).aspx

## A 2D Canvas Game

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
		margin: 0;
		padding: 0;
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

		last = Date.now()-16
		run()
	}

### Resize

	function resize()
	{
		canvas.width = width = window.innerWidth
		canvas.height = height = window.innerHeight
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
			move( pointersX[0] > centerX ? step : -step )
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

### Pointer Down/Move/Up

	function pointerUp( e )
	{
		setPointer( e, 0 )
	}

	function pointerMove( e )
	{
		setPointer( e, pointerLength )
	}

	function pointerDown( e )
	{
		setPointer( e, 1 )
	}

### Set pointer

	function setPointer( e, down )
	{
		e = e || event

		if( down < 1 )
		{
			if( pointerLength > 0 &&
				e.touches &&
				(down = e.touches.length) )
				return setPointers( e, down )

			pointerLength = 0
		}
		else if( e.touches )
		{
			pointerLength = e.touches.length

			for( var i = pointerLength; i--; )
			{
				var t = e.touches[i]

				pointersX[i] = t.pageX
				pointersY[i] = t.pageY
			}
		}
		else if( typeof e.clientX !== "undefined" )
		{
			pointerLength = 1
			pointersX[0] = e.clientX
			pointersY[0] = e.clientY
		}
		else if( typeof e.pageX !== "undefined" )
		{
			pointerLength = 1
			pointersX[0] = e.pageX
			pointersY[0] = e.pageY
		}

		e.preventDefault()
	}

### Keyboard Input

	{
		var D = document

		D.onkeydown = keyDown
		D.onkeyup = keyUp
	}

### Set key

	function setKey( e, down )
	{
		e = e || event
		keysDown[e.keyCode] = down
		e.preventDefault()
	}

	function keyUp( e )
	{
		setKey( e, false )
	}

	function keyDown( e )
	{
		setKey( e, true )
	}

### Schedule onresize events

	function scheduleResize()
	{
		if( resizeTimer )
			clearTimeout( resizeTimer )

		resizeTimer = setTimeout( resize, 200 )
	}

	document.onresize = scheduleResize

## Tips

### All numbers are floats!

http://seb.ly/2011/02/html5-canvas-sprite-optimisation/

	ctx.drawImage( sprite, x | 0, y | 0 )

### Object Properties and Array Indicies are slow

	object.a.b.c()

	array[0][0][0]

	// versus

	var a = object.a,
		b = a.b

	b.c()

Lookups take time. a[0][0] are two lookups. a[0] is just one.

http://moduscreate.com/javascript-performance-tips-tricks/

### Avoid allocations

	object = []

	object = {}

	new Object()

### Prerender

Draw frequently used elements in a Canvas element and draw this with
ctx.drawImage()

### Don't scale in the hot path

### Keep it simple!

IIFE (Immediately-Invoked Function Expression) doesn't make much sense
on a single page game.

http://benalman.com/news/2010/11/immediately-invoked-function-expression/

### Texture atlas

### devicePixelRatio / backingStorePixelRatio

	ratio =
		(window.devicePixelRatio || 1)/
		(ctx.webkitBackingStorePixelRatio ||
			ctx.mozBackingStorePixelRatio ||
			ctx.msBackingStorePixelRatio ||
			ctx.oBackingStorePixelRatio ||
			ctx.backingStorePixelRatio ||
			1)

	width = window.innerWidth*ratio | 0
	height = window.innerHeight*ratio | 0

	for( var i = 0; i < pointerLength; ++i )
	{
		pointersX[i] = pointersX[i]*ratio | 0
		pointersY[i] = pointersY[i]*ratio | 0
	}

### ctx.fillRect() versus ctx.clearRect()

	ctx.getContext( "2d", { alpha: false } )

The background of a 2D context is undefined. Undefined does not mean black.
So when we are using a 2D context with alpha set to false to speed up rendering,
it's important to fill the canvas explicitly.

https://jsperf.com/canvas-fillrect-clearrect/16 fillRect()

### Use two canvas on top of each other

### Avoid State-Changes

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

http://www.html5rocks.com/en/tutorials/canvas/performance/

## WebAudio

https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API
https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API/Using_Web_Audio_API

## Profile

Don't pre-optimize. Test, then optimize.

## Just do it!

## Ressourcen

https://hacks.mozilla.org/2013/09/getting-started-with-html5-game-development/

https://developer.mozilla.org/en-US/docs/Games

http://kodogames.com/optimising-html5-canvas-games/

https://hacks.mozilla.org/2013/05/optimizing-your-javascript-game-for-firefox-os/

https://simon.html5.org/dump/html5-canvas-cheat-sheet.html

http://www.html5gamedevs.com/

## Questions?

## Thanks!
