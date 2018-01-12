# Breakpoints
Media queries using the SASS / Susy framework are implemented using the "breakpoint()" mixin. 

There are different ways of intalling the breakpoint mixin.  I used a susy gem (see Zell's notes); then needed to refer to it in the "config.rb" file; finally, needed to import the sucker: 

```
@import "breakpoint";
```

## Notes on using the breakpoint mixin (syntax).

### Setting min-width
The following sets up a class...if greater than the min-width:

```
.red-above-600px{  @include breakpoint(600px) {  color:red; }}
```
This will render as follows:

```
@media (min-width: 600px) {  .red-above-600px {  color: red; }}
```

### Setting minimum and maximum width
This requires two argumnets.

```
.red-between-600px-and-900px {  @include breakpoint (600px 900px) {  color: red }}
```
This results in the following CSS:

```
@media (min-width: 600px) and (max-width: 900px) {  .red-between-600px-and-900px {color: red; }}
```

### Finally setting a max-width query
If the view port does not exceed the maximum width:

```
.red-below-900px{  @include breakpoint(max-width 900px) {color:red; }}
```

## Susy Breakpoint
The basic syntax for the Susy Breakpoint is:

```
@include susy-breakpoint($breakpoint-args, $context) {@content }
```

It is a convienence mixin that produces the following output:

```
@include breakpoint($breakpoint-args) {  @include nested($context) {    @content} }
```

### Example of Susy Breakpoint
Say you have an element that takes up 4 of 8 columns on a smaller viewport and 4 of 16 on a larger viewport, the SASS code (without the Susy mixin) will be:

```
$susy: (columns: 8);

.element {
// due to context this is 4 of 8 (i.e. default)
@include span(4);

@include breakpoint(1200px) {
	@include nested(16) {
		@include span(4)
	} // nested

} // include breakpoint

} // element

```
### Alternatively using the Susy Breakpoint
The could would be:

```
@include susy-breakpoint(1200, 16) {
	span(4)
} 

```
# Implementation
**Building a more responsive layout (page 110) **

## Preliminaries
Lets describe the dimensions of what we are actually trying to create.

### Mobile Dimensions
8 columns.  
Each gallery item takes up 4 columns. So, there are 2 items per row.

There are two widgets per row.    

### Tablet Dimensions
9 columns.
Each gallery item takes up 3 columns.  3 items per row.
Each widget takes up 4.5 rows. There are 2 items per row.

### Desktop Dimensions
The gallery section (i.e content) takes up 12 of the 16 columns. 

The sidebar section takes up 4 of the 16 columns and has a last (i.e no gutter to the right). 

The gallery items take up two columns.  But there is a 1 column (plus gutter) margin to the left and right of the gallery items.  So, there is (12 - 2) 10 columns available. That means that there is 5 items for each row. 

The widget items take up 4 columns.  That means there is 4 of them. 

## Actual Implementation

### The .wrap class
This defines a row. In HTML, this class is applied to: 

* Navigation
* Content and sidebar (as one unit) 
* Widgets

The wrap class is defined to include the Susy map using: 

```
@include container;

``` 
The .wrap class also defines a grid to assist in debugging for tablets
and for desktops.  These settings will overide the mobile first settings which are defined in the Susy map.

#### Navigation
This does not actually use Susy.

#### Content and Sidebar
On tablet and mobile the horizontal size of these fills up the entire viewpoint. But on desktop, content fills up 12 of 16 and then the sidebar fills up 4 of 16.  

These are defined as follows:


```
.content {
  @include susy-breakpoint($desktop, 16) {
    @include span(12);
  }
}

.sidebar {
  @include susy-breakpoint($desktop, 16) {
  @include span(4 last);
	}
}
  
```

#### The Gallery (page 120)
The **gallery class** is the unordered list and **gallery_item**
is each list item. 

Different breakpoints calculate different grids.  The first is for mobile:

```
 // Constrain span to only mobile  @include susy-breakpoint(max-width $tablet, 8) {    @include span(4);    margin-bottom: gutter();    &:nth-child(2n) {      @include last;    }}

```

The following is an explanation for the above:

We define a breakpoint with a context of 8 columns.  Then each item takes up 4 columns. After every second item, we do not put in a gutter.

The following is code for the desktop (tablet code ommitted -- see code for details)

```
// Constrain span to only desktop
@include susy-breakpoint($desktop,10) {
@include span(2);
margin-bottom: gutter();
	&:nth-child(5n) {
   @include last;
   }
}

```
The following is an explanation of the above:

Define a context of 10 (not 12 as per its container)

Each item takes up 2 columns...this means five per row.

#### Widget footer
We use the gallery mixin rather than the span mixin. The gallery add-in adds the nth-child piece of code automatically.

The context for the mobile is 8 columns, so the following:

```
@include gallery(4)

```
Defines that each widget takes up 4 columns.  This means (8/4) 2 per row.

We do not define separate CSS for the tablet version. This results in each item taking up 4.5 columns.










