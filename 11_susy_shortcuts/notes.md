# Susy Shorthand

The shorthand syntax is a shortcut to overwrite settings that are declared in the $susy map. It also provides a few extra arguments.

The shorthand has the following meta syntax:

```
shorthand: $span of $grid $location $keywords

```

Each set of arguments has its own rules.

## Span

This refers to the width of the element that we are creating.  It can take a length unit or unitless number.

## Grid
This refers to the columns and gutters of the grid that you are creating.

It is a combination of columns, column-width and gutters (optional) settings.

$gutters and column-width will use the settings within the global susy map if they are not passed into the mixin.

The following shows how to use $grid

```
// 12 columns, default gutters as defined in susy map
$grid: 12;

// 12 columns, 1/3 gutter ratio
$grid 12 1/3

```

The arguments can be grouped in brackets to tell Susy that certain things are grouped together.  This allows us to write asymmetric columns and gives room for us to write the column width.

```
// 60px columns and 10px gutters
$grid: 12 (60px 10px)

// asymmetrical grid with 5 columns and a .25 gutter ratio
$grid: (1 2 3 2 1) .25;

```

The $grid function is always preceded with the of keyword when used in a mixin or a function.

```
.grid {
	@include spand(3 of 4)
}

.grid2 {
	@include spand(3 of 6(20px 10px));
}


```

## Location
Location refers to the place where the span is supposed to be at.

It tells Susy to output the span in a specfic location.  These locations can be first, last or at a particular number.


## Keywords
There are two different types of keywords that we can use:

* Global Keywords
* Local Keywords

### Global Keywords
These are keywords that change the settings within the $susy map.

The keywords are shown below:

```
container: auto,
math: static | fluid,
output: isolate | fluid
container-position: left | center | right
flow: ltr | rtl
gutter-position: before | after | split | inside | inside-static
debug (image): show | hide | show-columns | show-baseline
debug (output): background | overlay

```
You can use any number of keyword combindations in your mixins or functions.

Keywords that are omitted will get their values from the global $susy map.

Here are two examples:

```
.wrap {
  @include container( auto static right ltr inside );
}

.span {
  @include span( isolate 8 of 12 0.5 at 2 split );
}


```

### Local Keywords
These are keywords that provide additional context for the mixin or function to create the correct value.  Here they are:

```
spread: narrow (default) | wide | wider,
role: nest
clear: break | nobreak,
gutter-override: no-gutters | no-gutter,

```

In regards to **spread** if it is set to *narrow*, Susy will include all internal gutters.  When set to **wide** Susy will include the width of one external gutter and it will include the width of two external gutters if set to **wider**



*The rest of the stuff is not noted...please see page 267 for details*












