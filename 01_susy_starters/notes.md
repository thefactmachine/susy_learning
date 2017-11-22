#Notes 
run "compass watch" to monitor and then compile. 

SASS has two dialects.  They are just different syntaxes.

The scss is compiled into css.

Every project begins with a $susy map which is just a key value structure. 

```
global-box-sizing: border-box  
```
Inside the suzy map makes sure that the boxe's size will be set to the outside of their border.

Remember that the box model goes from: content to padding to border to margin.

## Grid container
Every grid requires a grid container.  This is the div element with class wrap as shown below:

```html
<div class="wrap">
	<main class="content"><h2>Content</h2></main>
	<aside class="sidebar"><h2>Sidebar</h2></aside>
</div>
```

Then this is refferred to in the scss as:

```
.wrap {
  @include container();
  height: 100vh;
}
```

If the website is to be resposive, the container needs a 
max-width property instead of a width property.  Including the container as shown **above**, generates the following CSS:

```
.wrap {max-width: 960px;margin-left: auto;margin-right: auto;}
```

If we want the maximum width of the container to be 1140px, we can tell Susy to create this container by adding the container key to the $susy map.

## Last property
This just tells Susy that column was the final column and that means **do not** put a gutter to the right of the column.

## Context
$context is the argument that follows the **of** keyword.  In the following the context is 12.

```
@include span(3 of 4);
```
Context is also the number of columns in the parent element. 

### Rewriting our scss using parent context
Because the susy map is defined as:

```
$susy:(
  columns: 4, 
  container: 1140px,
  debug: (image: show), 
  global-box-sizing: border-box
  );
```

The context is 4 for the content and sidebar elements
So, we **had** our content and side bar defined as:

```
.content {
  @include span(3 of 4);
}

.sidebar {
  @include span(1 of 4 last);
}
```
But we can rewrite these as:

```
.content {
  @include span(3);
}

.sidebar {
  @include span(1 last);
}
```

## Nested layouts
We currently have a 4 column layout.  There are gutters after each of the first 3 columns.  We are going to define a column inside the first column.

Here is the HTML:

```
<div class="content">
<div class="inner-content">Nested Item</div>
</div>
```

Here we define the scss
```
.inner-content {
  @include span(1);
}
```






















