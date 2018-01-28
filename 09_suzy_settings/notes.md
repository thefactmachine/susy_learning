#Susy Settings
Upto now, we have covered 9 of the 13 available susy settings. So, we will export the remaning 4 settings.

The following are the default values.

```

$susy: (
  flow: ltr,                      // 1
  math: fluid,                    // 2
  output: float,                  // 3
  gutter-position: after,         // 4
  container: auto,                // 5
  container-position: center,     // 6
  columns: 4,                     // 7
  gutters: .25,                   // 8
  column-width: false,            // 9
  global-box-sizing: content-box, // 10
  last-flow: to,                  // 11
  debug: (                        // 12
    image: hide,
    color: rgba(#66f, .25),
    output: background,
    toggle: top right,
  ),
  use-custom: (                   // 13     
    background-image: true,
    background-options: false,
    box-sizing: true,
    clearfix: false,
rem: true, )
);

```

If you create a susy map, susy will overwrite the default settings above.   Another way to apply the same changes is to create a map of the setting you would like to use and then insert it by using the layout() mixin (as per the asymmetric grid).

## A small project to export the settings. 
The following is the html that we will be using:

```
<div class = "wrap">
	<main class = "content"><h2>Content</h2></main>
	<main class = "content"><h2>Sidebar</h2></aside>
</div>

```
### Flow (1)
Flow determines the direction which elements are being stacked. It only has two options either **ltr** (left to right) or **rtl** (right to left).  This may be used for langugaes that are written right to left.

### Math (2)
Math determines whether Susy outputs the CSS values in percentages or in the units specified in the column-width setting.  It has two possible options: **fluid** (default) or **static**.

*if you select static, make sure to set the column-width and to leave the container to auto*

### Output (3)
Output determines the layout technique used by Susy.  It takes two values: float and isolate.  Float is the most common technique but isolate is another technique which has been discussed extensively in chapter 14.

### Gutter position (4)
Where the gutter should be placed in relation to the columns.  This was coverted in detail in a chapter on gutter positions. 

### Container (5)
It takes two values:  auto and length (in any value unit measurement).

Container sets the maximum width of the container.  Set this to your preferred max-width if you are working with fluid math.

Leave it to auto, if you are using static math.


### Container Position (6)
Determines whether the container is flushed to the left, centered, or flushed to the right of the page -- it sets the margin-left and the margin-right property for the Susy container.

The default value is *center* and this basically writes css code of auto for margin-left and margin-right. 

Other values are: left, right and length.

Left and right will cause the container to be flushed left or right. The following is the css generated for "left"

```
margin-left: 0;
margin-right: auto;

```

The length argument means that you can give the container-position key a length with any valid CSS unit.  This length will be given to both margin-left and margin-right.

### Columns (7)
Columns define whether you are using a symmetric or an asymmetric grid.  Columns defaults to 4 and takes either a number (symmetric) or a list (asymmetric) grid.

### Gutters (8)
Gutters has the number 0.25 as default but it can be specified as a ratio.  Gutters are calculated with a ratio of "gutter / column-width" If you want to remove gutters, set it to 0.


### Column Width (9)  
The default value for this attribute is "false". When the value is false, it tells Susy to calculate the size of the columns depending on the gutter ratio and the number of columns in the layout -- this will mean that the column width will be in percentages.

It is only important to calculate column-width when using a static grid. 

### Global Box Sizing (10)
There are two options: "content-box" and "border-box".  We have been using the latter.

### Last Flow (11)
The author claims that this no reason to really adjust this setting. It has two values: "to" and "from". "to" is the default. 

This mitigates sub-pixel rounding errors by floating the final element in the opposite direction from the rest of the elements in the row.  For example, in a Left to Right flow, the final element (the rightmost) is floated left while the other preceding elements are floated left.  

However, if using "from" then the last element will be floated in the same direction as the previous elements. 

### Debug (12)
This has four sub keys (sub options).  

These are as follows:

```
$susy: (
debug: (
image: hide, (hide | show | show-columns | show-baseline)
color: rgba(#66f, .25),
output: background, (background | overlay)
toggle: top right, (left | right and top | bottom)
)
);

```

The option *image* provides a range of display options.  When the *output* option is set to "overlay" a toggle appears in the top right of the screen and then the user can press on this to turn the grid on and off.  The option *toggle* allows you to change the location of the toggle. 

### Use Custom
Can really see any use for this.  Not studied. 







