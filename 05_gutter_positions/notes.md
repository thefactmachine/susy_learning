#Gutter positions
Upto now, we have only used the "after" gutter position.  This makes us define a zero gutter position at the rightmost column.

But there are also: **before , split , inside and inside-static gutter** positions. 

The code for all four examples is in the folder supplied. 

The html is a bit tricky because things go down 2 levels deep.

##### The structure in words

Have a look at the image by opening up the index.html file.  Basically, we have two columns:  content (6 of 8) and sidebar (2 of 8).

Content contains two rows.  The first row is split into three columns (2 of 8).  And the second row occupies the entire width (of the content section).

The first row of content (called gallery items) [as mentioned] as three columns.  The first two columns contain 2 sub items. Both of these occupy 1 column in width. 

## After
The following shows the structure for content that **excludes** the sub-items. This is wrapped inside content

```
<div class = "content">
	<div class = "gallery">
		<div class="gallery__item"></>
		<div class="gallery__item"></>
		<div class="gallery__item"></>
	</div>
</div>
```
The following shows the scss for gallery_item:

```
.gallery__item {  @include nested(6) {    @include span(2);    &:last-child { @include last; }	} 
}

```
The scss for content is as follows:

```
.content {
  @include span(6);
}

```

And finally, here is the code for sub-item:

```
.gallery__subitem {
  @include nested(2) {
    @include span(1);
    &:last-child { @include last;}
  }
}
```
Both gallery and gallery_item have clear fixes.

## Before
We firstly need to modify the Susy map with the following key:

```
gutter-position: before

```

Now, we have to remove the gutter from the first element.  Previously, we applied last to the side bar.  Now, we apply first to the content:

```
.content {
  @include span(6 first);
}


```

So, here is the scss for the gallery item:

```
.gallery__item {  @include cf;  @include nested(6) {    @include span(2);    &:first-child {      @include first;    }} 
}
```
For the gallery, sub-item, the first element is an H2 tag, so we use the nth-child selector as follows:

```
.gallery__subitem {
  @include nested(2) {
    @include span(1);
    &:nth-child(2) {
      @include first;
    }
  }
}
```
## Split
With split, the gutters are divided into two and placed on each side of the column. 

The **fundermental** difference is that parent columns do not have any gutters.  So they become flush with each other. Only the child elements align nicely with the grid. 

The implement this, parent elements must have a nest argument. 

### Implementation
Both content and sidebar are parent elements, so they need a nest element.  This is shown below:

```
.content {  @include span(6 of 8 nest);}.sidebar {  @include span(2 of 8 nest); // We are assuming .sidebarcontains spanned child elements}

```

With the gallery-item, the third item is not given a nest because it does not have child elements:

```
.gallery__item {  @include cf;  @include nested(6) {    @include span(2 nest);    &:nth-child(3) {      @include span(2)    }} }

```

### Full
Full is also a child element. We give it a span, so that it does not extent outside the grid:

```
.full {  @include span(full);}
```
## Inside
Inside and Split are the same.  Except that Inside outputs padding rather than margins. With padding there are no gutters (i.e gaps at all). 


### Clearfixes
If all the child elements are going to be floated, then a clearfix is needed. 


