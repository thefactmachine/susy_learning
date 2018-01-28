# Multiple Susy Grids
We will create a page where we use the 3 types of Susy grids that we learnt about: fluid, asymmetric and static.

## Global and Local Scopes
When a variable is defined outside of a selector, we will call it a global vairable.  The following shows a local variable:

```
// this is a local variable
.testing {
	$var: green;
}
```

### Global and Local Scopes with Susy
The global variable in Susy is the $susy map.  Once the $susy map is defined, it is used anywhere when a Susy function or mixin is used.

There are two ways of creating local scope with Susy:

See the comments below:

```
$susy: (
columns: 12
);
// Context of span is 9 instead of 12. So, columns = 9 is 
// the local scope.  But this cannot be re-used.  
.span {
@include span(3 of 9);
}
```

The local scope of columns = 9 cannot be reused.  So, we can used the nested() mixin in order to create a re-usable local scope:

```
$susy: (
columns: 12
);
// Local scope of 9 columns apply to everything within the
nested mixin
@include nested(9) {
@include span(3); // same as @include span(3 of 9)
margin-bottom: gutter(); // same as gutter(9)
}

```

### The with-layout() mixin
We can apply the same method to create multiple grids.  However, we use the with-layout() mixin instead of the nested() mixin.

The with-layout() mixin takes in a map argument and overwrites the $susy map with every setting given to it.

## An example.
Here is the html:

```
<section>
	<h2>Fluid Grid</h2>
	<div class="fluid-wrap"></div>
</section>
	
<section>
	<h2>Asymmetric Grid</h2>
	<div class="asym-wrap"></div>
</section>

<section>
	<h2>Static Grid</h2>
	<div class="static-wrap"></div>
</section>

```

Apart from other stuff, we create 3 different maps by varying columns ...etc:


```
$fluid-grid: (
container: 1140px,
columns: 12,
gutters: 0.25
);

$asym-grid:(
output: isolate,
container: 1140px,
columns: 1 2 3 4 5 6,
gutters: 0.1
);

$static-grid: (
math: static,
column-width: 80px,
columns: 8,
gutters: 0.5
);

```

And then we apply our work:

```

.fluid-wrap {	@include with-layout($fluid-grid) {		@include container;	}}.asym-wrap {	@include with-layout($asym-grid) {		@include container;	}}.static-wrap {	@include with-layout($static-grid) {		@include container;	}}

```

Now we have three different grids laid out on the same page.

Cannot find the source code on github.

## Susy Shorthand
Say we wanted to create a different layout that has 8 columns, 0.5 gutters and a split gutter position, we can do this with the following code:

```
.onthefly1-wrap {
	@include with-layout(8 0.5 split) {
	@include container;
	}
}
```















