# How this runs outside Khan Academy

The game code is unchanged. Everything that made it work on Khan Academy has
been rebuilt on top of an HTML canvas.

## Why not Processing.js

Processing.js was the obvious candidate and the wrong one. It was abandoned
in 2018, and more importantly its trigonometry works in radians while Khan
Academy's variant works in degrees. Every `rotate()`, `sin()` and `arc()` in
these programs assumes degrees. Processing.js also runs a preprocessor over
the source that rewrites `new`, which is the entire reason some of these
programs carry a hand-written object-construction helper.

So `ka.js` is a purpose-built shim: canvas 2D, degrees, no dependencies.

## What the shim provides

Drawing (shapes, curves, beziers, custom polygons), the transform and style
stacks, text and fonts, images and pixel grabs, colour with Processing's
packed-integer representation, Perlin noise, the maths helpers, keyboard and
mouse state, and Khan Academy's audio calls.

Constants are Processing's real numeric values — `LEFT` is 37, `CENTER` is 3
— because programs index their own arrays with them. A program that writes
`clicked[mouseButton]` and later tests `clicked[LEFT]` only works if both are
the same number.

## Known limits

**`filter()`** supports blur, invert and greyscale. Other modes are ignored
rather than approximated.

**Sound** is silent unless you supply audio. Khan Academy hosted its own
sound library; those files are not redistributed here. Drop matching `.mp3`
files into `sounds/` using the same names the program asks for — a call to
`getSound("rpg/hit-thud")` looks for `sounds/rpg/hit-thud.mp3`. With no files
present the game runs normally and stays quiet.

**External images** cannot be fetched. These programs draw all their own
artwork in code, so this rarely matters.

**3D calls** are accepted and ignored. Nothing here draws in 3D; the names
exist so that programs referencing them do not fail to load.

## Canvas size

Khan Academy let you change the canvas by adding `?width=` to the URL, and
several of these programs check the result and refuse to draw at the wrong
size. The size lives in `config.js`. If the layout looks wrong, that is the
file to edit.

## Differences from the original

Where the original had a bug that visibly affects play, it has been left
alone — the game is the game it was. Anything genuinely changed is marked in
`game.js` with a comment.
