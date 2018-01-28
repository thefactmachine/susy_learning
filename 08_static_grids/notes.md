# Static Grids
Page 223

## Fluid and Static Layouts
Fluid layouts use percentages. This allows the web page to stretch and contract relative to the user's screen size. Static or fixed layout uses a preset page size and does not change based on the browswer width. Static grids are defined in units rather than percentages. 


## The HTML
Here is an outline

```
<div class = "wrap">
	<div class = "content"><h2>content</h2></div>
	<div class = "sidebar1"><h2>sidebar 1</h2></div>
	<div class = "sidebar2"><h2>sidebar 2</h2></div>
</div>

```

## Setting the container
This is done as follows:


```
.container {
	width: 400px;
	
	@include breakpoint(700px)  {
		width: 600px
	}
	
	@include breakpoint(1100px) {
		width: 900px
	}

}

```


## The Suzy Map
We need to set the "math" to static and ensure that the container is set to auto.

The **column-width** key determines the size of a column.

In the mobile layout there will be 4 columns. And the gutter size will be 15 px.

Here is the map:

```
$susy: (
	columns: 4,
	column-width: 90px,
	gutters: 15px / 90px, // or 0.16666667
	math: static,
	global-box-sizing: border-box,
	debug: (image: show)
);
```
## Coding the wrap
As per normal, we define the wrap as:

```
.wrap {
	@include container;
}


```

When translated to CSS, this ends up as:

```
.wrap {
	width: 405px
	margin-left: auto;
	margin-right: auto;
}
```
Suzy has calculated the width of (4 * 90) + ((4 -1) * 15)

And this adds up to 405px.

See code 17-1.

## Creating the layouts
The following sections step though how to create each of the different sizes.

### Small layout.
Well nothing really has changed here.

### Medium layout
This is how we define the container:


```
$med: 8;
.wrap {
	@include container;
	@include susy-breakpoint($bp-med, $med) {
		width: span($med);
		@include show-grid;
	}

}

```
The above matches the width of an 8 column layout.  The width is rendered into CSS as:  720 + 105  = 825


Laying out the rest is pretty easy:

```
.content {
	@include susy-breakpoint($bp-med, $med) {
		@include span(4);
	}
}

.sidebar1 {
	@include susy-breakpoint($bp-med, $med) {
		@include span(2);
	}
}


.sidebar2 {
	@include susy-breakpoint($bp-med, $med) {
		@include span(2 last);
	}
}


```

See code 17.2

### Large Layout
Its basically identical to the medium layout.  See 17.3







