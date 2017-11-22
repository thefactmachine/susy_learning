# More Complex Example (Part 2)

## Gallery Component
We need to style the gallery-item element which is a list element

We want to display 5 images within 10 columns.  This means that each image is 2 columns.  So initial we include the following:

```
.gallery__item {  @include span( 2 of 10 );}
```

But using the snippet above fails to remove the right margin every 5th item.  

To remove the margin-right of every 5th item, we need to use the nth-child pseudo class.

```
.gallery__item {  @include span( 2 of 10 );  &:nth-child(5n) {    @include last;  }}
```
This renders as follows:

```
.gallery__item:nth-child(5n) {
  float: right;
  margin-right: 0;
}

```

## Making the bottom margin the same as the gutter

Easy to do this, we use the horizontal gutter as the vertical gutter.

We know the context is 10 columns, so just do this:

```
  margin-bottom: gutter(10);
```

## Make the code dryer using the nested mixin
All the above can be written as:

```
@include nested(10) {
  .gallery__item {
    @include span(2);
    margin-bottom: gutter();
    &:nth-child(5n) {
      @include last;
    }
  }
}
```

## SCCS For the Widget
```
.widget {
  background: rgba(240, 150, 113, 0.8);
  @include span(4 of 16);
  &:nth-child(4n) {
    @include last;
  }
}
```
All the above can be re-written as:

```
.widget {
  background: rgba(240, 150, 113, 0.8);
  @include gallery( 4 of 16 );
}
```
The gallery mixin creates such different CSS because it uses a technique called the **Isolate Technique**

This technique can avoid subpixel rounding errors.  

## Nth Child
```
p:nth-last-child(3n+0)
```
This means count from the last child (offset 0) every third element.






