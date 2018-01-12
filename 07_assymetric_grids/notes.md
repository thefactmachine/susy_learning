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
The following describes the content from the start up until "about  Carol Twombly"

Everything is surrounded by a div wrap class. This includes the section "about Carol Twombly".

The first sections up until "Carol Twombly" are surrounded by div "content intro"

The title "This is Chapparal" is an H1 with a class of "title".

The sub-title "Created by Adobe type ..." is h2 with the class of "sub-title"

The first paragraph "The result is a versatile.."  is a div style "content_left".  This is further wrapped in p tage.

The second paragraph "Like a drought-resistant" is a div style "content__right."  This is further wrapped in p tage.

### Notes about the mobile version
The heading take up two of the two columns.  The relevant SCSS seems to be:

```
.intro {
  .content__left, .content__right {
      @include span(full);
    }
}

```
The above means use the entire two columns.  As everything is basically inside an intro class, then the h3 element gets the two columns.  [need to check specificity settings to see why the actual content text gets the right column (i.e the second column...).  

The SCSS for the content_left and content_right have the follwing SCSS:

```
.content__left,
.content__right {
  clear: both;
  @include span(1 last);
}
```

So, this occupies only the second column.