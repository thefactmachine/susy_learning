#Isolate Techniqe
This is a technique to address sub-pixel rounding errors. And we use it to create grid based layouts.

The key feature of this technique is that it uses **negative margins**.  In particular, it uses a **negative right margin** and this pulls the *suceeding column* completely over to the left.  And then a conventional left margine pushes this column over to the appropriate place on the left.

## An example of isolate

A snippet of code might make things clear.  This is the first column:

```
.widget:nth-child(4n + 1) {
	margin-left: 0%;
	margin-right: -100%;
	clear:both;
}

```

And this might be the second column:

```
.widget:nth-child(4n + 2) {
	margin-left: 25%;
	margin-right: -100%;
	clear:none;
}

```

In the above, column 2 is pulled completely back to the starting line by column 1.  The right margin on column 2 is to pull column 3 (not shown) competely back to the starting line. 

## Breaking to the next line
If you want an item to break onto a new line, it has to use the following.

```
clear:left

```

If the row needs to be reset back to the left, then it needs to have a *margin-left: 0%*

#Implementing isolate using Susy
We will be making a simple page using **three** Susy techniques.

## Description of the example
We will make a moble (4 column) and a desktop (12 column) layout. There are three blocks.  The first of these is called content and on a mobile it occupies the first row (4 columns) On mobile, the next two blocks are called *sidebars* and they are put on row 2 occupying 2 columns each.

On the desktop, the layout is all on one row in the following order sidebar1 (2 columns) content (8 columns) sidebar2 (2 columns).


## Method 1 - Global Isolate
This method sets a global option in the Susy map to isolate instead of float. This means that all the span mixins will use a $location argument.  The following shows, the global declaration in the Susy map:

```
$susy : (
output: isolate,
// ...
)
```
The following shows the CSS for the mobile and desktop layouts.  Comments have been included for extra explanation:

```
.sidebar1 {
  @include span(2 first);

  @include susy-breakpoint(960px, 12) {
    @include span(2 first);
  }
}

.sidebar2 {
  @include span(2 last);

  @include susy-breakpoint(960px, 12) {
    @include span(2 last);
  }
}

.content {
  @include susy-breakpoint(960px, 12) {
      // for the desktop, content spans 8 columns and this starts
      // at column 3.  content occupies the entire first row for the mobile
      // layout, so we do not need to add any code.   
       @include span(8 at 3);
  }
}

```

### Warning
If there is no location argument found then Susy will treat the item as a normal float element. 

## Method 2 - Add Isolate Keyword
The second method is to add the *isolate* keyword to the span mixin.  This works even if the global argument is set to *float*  This method is exactly the same as the previous method except an isolate keyword needs to be used. 

The following repeats the code above (from sidebar 2) and shows how to use prepend the isolate keyword.

```
.sidebar2 {
  @include span(isolate 2 last);

  @include susy-breakpoint(960px, 12) {
    @include span(isolate  2 last);
  }
}

.content {
  @include susy-breakpoint(960px, 12) {
  @include span(isolate 8 at 3);
  }
}

```

This second method allows you to use float for most of your site and the isolate for target portions.

## Method 3 - Isolate mixin
The isoldate mixin takes the same arguments as the span mixin but it does not **output width**

If you use this method, you need to manually put in the width for the item that you are trying to isolate. You can use the span element for this.  However, the *isolate* mixin cannot calculate last as there is no way that it knows the width of the element and hence the precise location. 

For the content item above the scss would be:

```
.content {
  @include susy-breakpoint(960px, 12) {
  width: span(8);
  @include isolate(3)
  }
}

```

## Another row of elements
See notes on page 182.


