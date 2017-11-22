# A More Complex Example
We removed margins and paddings for all <ul>, <ol> and <li> elements.

Also removed list styles from all <li> elements.   

Finally, added max-width: 100% and height: auto to make images responsive.

## The import statement
This just imports an existing SASS file, so you can reference it.  When you
import the file you dont need the file extension .scss.

## The Susy Map
Sets up main parameters of the project.

```
$susy: (columns: 16, 
container: 1140px, 
global-box-sizing: border-box, 
debug: (image: show));

```

## The header section
This does not use any Susy constructs.  There are five menu items.  These
are on the right hand side of the top menu.  Their container (i.e. <nav> ) is floated right but each of the elements is floated left insider their container.

### Navigation Bar to be 100%
We put everything inside a div element with the general "wrap" class.

 
## The Sidebar and Content
There is a main div with a class "wrap" that contains both these columns.  The main content is enclosed in a <main> element and the sidebar content is enclosed in a <sidebar> element.  The susy CSS for these is:

```
.content {
  @include span(12 of 16);
}

.sidebar {
  @include span(4 of 16 last);
}

```
## The Clearfix Mixin
The code for this is as follows:

```
@mixin cf {
  &:after {
    content: "";
    clear: both;
    display: table;
  }
}
```
The "&" refers to the parent when the mixin is implemented.

"If the parent element contained nothing buth floated elements, the height of
it would literally collapse to nothing...this isn't always obvious if the parent does not contain any visually noticeable background"


The mix in above is implemented by using the following:

```
.gallery {
  @include cf;
  }
```

And as a consequence, renders as follows:

```
.gallery:after {
  content: "";
  clear: both;
  display: table;
}
```

The galley is the parent container for the gallery-item list item.

## Difference between functions and mixins
Functions return a value; mixins are blocks of code.

### span-mixin example
```
@include span(12 of 16)

```
### span function example
```
width: span(12 of 16)
```
## $spread Argument
This has three different options: narrow, wide and wider.  Narrow does not include gutter widths.  Wide includes 1 gutter.  Wider includes 2 gutters.  These gutters are in addition to the column width. 


## Gutter Function
In a container of 8 columns, one gutter size would be gutter(8).

## CSS for Gallery Item
What we want to do is to define the gallery to 10 columns within a 12 column context.  And then we centre it.

All the following, is in the context of defining the class for "gallery"

Here we set the width to 10 columns:

```
width: span(10 of 12);
```
Now there are three different methods of centering:

### Using Margin Auto.
This makes the left and right margins, equal width:

```
  margin-left: auto;
  margin-right: auto;
```
### Using the spread argument:
The following defines the margin-left as 1 column + 1 gutter
```
margin-left: span(1 wide of 12);
```

### Using the gutter function
```
margin-left: span(1 of 12) + gutter(12)
```









