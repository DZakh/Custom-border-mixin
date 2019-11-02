# Custom border mixin

This mixin is an ultimate solution if you need very customizable dashed or dotted borders for your project. You can tune sides of borders, dots' size, dashes' length, the gap between them, as well as color, and starting point which allows you to create magnificent animations.

### [Demo with code examples](https://codepen.io/dzakh/pen/NWWwRpp)

## Getting started

Install the latest version with NPM.

    npm install custom-border-mixin -D
Then import the mixin after vendors in your project.
```scss
@import 'node_modules/custom-border-mixin';
```

That's all! Or instead you can barbarously copy everything from the `index.scss` file and paste this somewhere in your project.

## Create your first border
To make a border you need to write this line in your sass stylesheet:
```scss
@include custom-border( dotted() );
```
We did it! This will create a simple dotted border with default properties. But I think that's not enough for you. Let's make something more crazy:
```scss
@include custom-border(
	dotted(_, 1px, 4px, red),
	dashed(horizontal, 3px, 6px, 6px, blue, _, opposite),
	dashed(top, 1px, 2px, 2px, black),
	dotted(_, 1px, 4px, pink, 0.5)
);
```
Honestly I don't really know how it looks, but let's figure it out. As you can see the mixin `custom-border()` can take functions either `dotted()`, or `dashed()` as parameters. Everything should be built with these functions, and if you call another mixin `custom-border()` it will clear all borders initialized before.

## Tuning borders

Let's take a look at `dashed()` function closer. I know there's also `dotted()` one, but it's literally the same just without length property.
```scss
dashed($side, $size, $length, $gap, $color, $translate, $start)
```
Ok, one by one:  
**`$side`** (string)  
Should be either **top**, **right**, **bottom**, **left**, **vertical**, **horizontal**, or **all**.  
**`$size`** (number)  
Should be any non-negative size number. Represents size of dots or thickness of dashes.  
**`$length`** (number)  
Should be any non-negative size number. Length of dashes.  
**`$gap`** (number)  
Should be any non-negative size number. Length of empty intervals.  
**`$color`** (color)  
Should be a color. It's color.  
**`$translate`** (anything)  
Can take any value. If false, will not change anything; if true, will move the border one section; if number, will take the number as a coefficient and move the border from the original position.  
That's a very useful property for fine-tuning borders and also for animations.  
**`$start`** (string)  
Should be either **origin**,  or **center**,  or **opposite**. These are like justify-content's values: start, center and end. For better understanding take a look at [code example](https://codepen.io/dzakh/pen/NWWwRpp).  

> **Note:** translate property doesn't work with **center** value.

*Also, you can use the underscore sign `_` to skip the property and keep it as default.*

### Border defaults

Since I touched on the topic of default values let's delve into it. 

All defaults are initialized by map variable and you can easily change them. You just need to create the same `$border-default` variable above the imported mixin.
Here is the default `$border-default`:
```scss
$border-defaults: (
	'side': 'bottom',
	'size': 1px,
	'length': 1px,
	'gap': 1px,
	'color': #000,
	'start': 'origin',
	'translate': false
) !default;  
```
> **Note:**  be sure not to forget anything, because it won't work without all these properties.



## Animation

Finally we are here. Let's make it move with `@keyframes`. You can also use `transition` for it, but I'm going to keep it for you.
```scss
@keyframes border-animation {
	from {
		@include  custom-border( dashed(bottom, 1px, 3px, 11px, red) );
	}
	to {
		@include  custom-border( dashed(bottom, 1px, 3px, 11px, red, true) );
	}
}
```
Very simple animation. It moves the border one section counterclockwise — to the right in our case. Turn it on:
```scss
.item-with-super-cool-border {
	/* Some styling */
	animation-name: border-animation;
	animation-duration: 1s;
	animation-iteration-count: infinite;
	animation-direction: normal;
	animation-timing-function: linear;
}
```
You can make a lot of cool animations playing around with translate property. Use number coefficients to make smooth transitions between sides, and use negative values to turn specific border to different direction. [More animation examples on CodePen](https://codepen.io/dzakh/pen/NWWwRpp).

> **Note:** be sure that both of your values have the same decimal part to prevent unexpected jerking behavior.

## Known issues

 - Since this mixin is built on border-image property, you should create pseudo element to use it with your own border-image.
 - And you cannot use border-radius property.

## Let's wrap it up

I  made this mixin only fortnight after first acquaintance with sass. Now I'm in love with this preprocessor, but I would not be able to make this mixin so customizable, if there were no [Empty-state mixin](https://github.com/wildhaber/empty-state) that was kind of a mentor for me during my path.

I hope you find my mixin useful in your work \^_^
