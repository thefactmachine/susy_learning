#Assymetric Grids
Starts from page 184.

These are grids which have unequal column widths. 

Assymetric grids use the **isoldate** technique to define column widths, so we use the *isolate* in the column header. 

```
$susy: (
output: isolate
// .....
);
```

## Columns setting
Susy will create an assymetric grid if a SASS list is given to the columns property within the $susy map.

The following is an example:

```
$susy: (
columns: 0.25 1.00 2.00
);

```
Within the list assigned to the columns property there **must be a base column** with the **value of 1**


## Gutter settings
The gutter is based on the value of the base column. For example, in the above, if the base value is 20px and we want a gutter of 10px then the gutter is defined as follows:


```
$susy: (
columns: 0.25 1.00 2.00
gutters: 10 / 20;
);

```

## Example -- creating the small layout
In this example, we have the following:

* Column 1 is 42 px
* Gutter is 10 px
* Column 2 is 288 px

It is sensible to use the smallest column as the base multiple. So, doing this results in the following calculations:

* Column 1: 42 / 42 = 1
* Gutter: 10 / 42 = 
* Column 2: 288 / 42 = 6.857

We need to set the column width to 42.  The resultant map is the following:

```
$small: (
	columns: 1 6.857,
	gutter: 10 / 42,
	column-width: 42px 
)

```
## Medium and Large Designs
For these designs the author does not set the column width but instead the columns are set based on the pixel size of the browser width.

Within the notes, for the **medium design** the author uses pixel sizes but these are based on a nominal browser width of **602 pixels**.  

In this design, the author uses 4 columns and they have the following nominal width:  54, 88, 370, 54.  There are 3 gutters of 12 pixels.  These all sum to 602 pixels.

The **break points** for medium is set at 530 pixels.  And the break point for large is set at 700 pixels.  So, the column proportions for the medium design will be within this 530 ~ 700 pixel range.  They will only equal the precise amounts described above when the browser width is set to 602 pixels. 

### Precise proportions for the medium layout
The medium layout has the following precise proportions:

* Column 1:  1 (54 / 54)
* Column 2:  1.629 (88 / 54)
* Column 3:  6.851 (370 / 54)
* Column 4: 1 (54 / 54)
* Gutter:  12 / 54

So, lets check with Pixel Window and set the browser to 690.  Now, the largest column, should be about 424 pixels.  (this is approximate...there is currently a bit of confusiion about the getting browser widths... and column sizes with PixelWindow.

## Applying the maps
In order to create the susy container with the smallest map, we have to use the layout mixin to substitute the default map with the one that we want. This is done with the following code:

```
@include layout($small);

```
This overwrites the map settings with the values defined in $small.  

This is how we set the medium and large breakpoint:

```
.wrap {
  @include container;
  @include susy-breakpoint($bp-med, $med) {
    max-width: 100%;
    @include show-grid;
  }

  @include susy-breakpoint($bp-large, $large) {
    @include show-grid;
  }
}

```
The following is an example of setting the $med map:

```
  columns: 1 1.62963 6.85185 1,
  gutter: 12 / 54
  );

```
#Assymetric Grids (part two)
*This is page 204.*

## Understanding the HTML
The following describes the content from the start up until "about  Carol Twombly"  There are three sections with basically the same format.  Here is an outline of the three sections.  The detail within each section is **not** shown below:


```
<div class = "wrap">
	<div class = "content intro">  content here </div>
	<div class = "content">  content here </div>
	<div class = "content">  content here </div>
</div>

```

The following is the basic structure for a single section.  The content__left and content_right are used for large layout.  When they are layed out using a multi-column type layout.

```
<div class = "wrap">
	<div class = "content intro"> 
		<h2> header section here </h2>
		<div class = "content__left">  text goes here  </div>
		<div class = "content__right"> text goes here </div>
	</div>
</div>

```


## Notes on context
We can ignore the strings of decimal points when creating the asymmetric grid.  All we need to know is the **number of columns** in each layout. 

*  $small has 2 columns
*  $medium has 4 columns
*  $large has 5 columns


### SCSS for content_left and content_right
It is the following:

```
.content__left, 
.content__right {
	clear: both
	@include span(1 last);

}

``` 

This generates the following CSS:

```
float: left;
margin-left: 15.41851%;
margin-right: -100%;

```

So, the above seems to set content-left and content-right to a single right-most column.

Since, the content on the intro takes up the two columns, we can create a selector with a higher
specificity to override what we have already declared.

```
.intro {
	.content__left,
	.content_right {
		@include span(full);
	}
}

```

The above results in the following CSS:

```
@media (max-width: 529px) {
  .intro .content__left, .intro .content__right {
    width: 100%;
    float: left;
    margin-left: 0;
    margin-right: 0; } }

```


We now give .content a clearfix since all its child elements are floated.

.content {
	@include cf;
}

--------------
## Medium Layout
Firstly we need to define the outer .wrap class:

```
.wrap  {
	@include container;
	@include susy-breakpoint($bp-med, $med) {
		max-width: 100%;
		@include show-grid;
	}
}

```

The medium grid has 4 columns.  

The content spans the two middle columns and is starts on the second 
column:

```
.content {
	@include cf;
	@include susy-breakpoint(530px, $med) {
			@include span(2 at 2);
	}
}


```

### Getting the columns value from a specific context:

The following gets the columns key from the map called $med.

```
$columns: map-get($med, columns)

```
Then, we use @debug to log out the value $columns to see if we have the correct value.

```
@debug $columns; // returns: 1, 1.62, 6.85, 1
```

Next, we need to find the context.

We can this context with the nested() function that Susy provides.

```
$context: nested(2 of $columns at 2);
@debug $context; // returns 1.62 6.85

```

Now, we can use this context within the span() mixin.

```
.content-left,
.content-right {
	clear: both;
	@include span(1 last);
	
	@include breakpoint($bp-med) {
		// get the columns key from the $med key / value map
		$columns: map-get($med, columns);
		$context:nested(2 of $columns at 2);
		// I have not idea why this is "1 of..."  I would have thought it would have been
		// two....
		@include span(1 of $context last);
	}
}


```

We can further shorten things up...quite a bit:

```
// the following two lines:
$columns: map-get($med, columns);
$context: nested(2 of $columns at 2);
// Can be replaced by:
$context: nested(2 of map-get($med, columns) at 2);

```

### Targeting the smallest layout
The intro pargraph is only different on the smallest breakpoint.  The max-with media query will get activated when when it is **less than** the parameter given.  In the following, it will be activated when the viewport is **less than** $bp-med - 1.

```
.intro {
	.content-left, content-right {
		@include breakpoint(max-width $bp-med -1) {
			@include span(full);
		}
	}
}

```

## Large Layout
The large layout has a 5 column setup.  We change the grid accordingly in the .wrap section (not shown).

The .content now spans 3 columns in the $large layout and it starts on the second column. 

This is how we code this:

```
.content {
	//......
	@include susy-breakpoint($bp-large, $large) {
		// span 3 columns and start it at the second column
		@include span(3 at 2)		
	}
}

```

Now we add the bit for .content-left and .content-right

```
// see code 16-3
.content-left, content-right {
	// ..
	@include breakpoint($bp-large) {
		// map-get extracts the columns key from the $large map
		// we get 3 columns started from column two
		@include nested(3 of map-get($large, columns) at 2) {
			// start at the second column...??  this bit is a mystery...
			@include span(2 last);
	}
}

```


## Extra Large Layout
The background grid is the same as for the large layout.  The only thing that has changed is the .content-left and content-right takes up 1 column.  The left takes up the 3rd column and the right takes up the 4th column. 

### Nested Grid Items
.content-left and content-right are nested grid items.  Not sure exactly what this means.

The CSS for .content-left is:

```
.content-left {
	@include breakpoint($bp-xlarge) {
	 @include nested(3 of map-get($large, columns) at 2) {
	 	@include span(1 at 2);
		}
	}
}

```

The CSS for .content-right is as follows. Both .content-left and content-right have a clear:both property built into them.  This clear:both is causing the .content-right to create a new row for itself.  Since we want both of them to be on the same row, we need to remove this clear: both property from .content-right.


```
.content-right {
@include breakpoint($bp-xlarge) { 
	clear:none
	@include nested(3 of map-get($large, columns) at 2) {
		@include spand(1 last);
		}
	}
}

```

The code above can be made dryer by combining the .content-left and content-right selectors.  See the example. 

See the code 16-4.







