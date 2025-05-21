what is CSS ? Cascade Style Sheet
Language To create The presentation And Visuals , CSS has already 3 levels
```HTML
<html lang = "en">
	<head>
		<meta charset = "UTF-8">
		<title> CSS </title>
		<link rel = "stylesheet" href = "CSS file path">
	</head>
	<body>
		<p> This is paragraph </p>
	</body>
</html>
```

How to style an element using CSS
```pusdocode
Selector {
	Property1: value1;
	Property2: value2;
}
```

for example this is the code to style p element , we can also instead of styling an element we can style a class then apply this class to any element we want
```CSS
p {
	color : red;
	font-size : 40px
}
```

we created a class my-p , we need to add dot before class name , to declare this is a class , or we can define an id with the same way but putting # before the name
```CSS
.my-p1{
	color : red;
	font-size : 40px
}
#my-p2{
	color : red;
	font-size : 40px
}
```

```html
<p class = "my-p1"> This is paragraph </p>
<p id    = "my-p2"> This is paragraph </p>
```

comment in CSS
```CSS
/* multiple line comment
we can use  in css
*/
```

we have 3 ways to call CSS file
```html
<html>
	<head>
		<!-- External Style -->
		<link rel = "stylesheet" href = "">
		<!-- internal Style -->
		<style>
			/* CSS code */
		</style>
	</head>
	<body>
		<!-- inline style -->
		<p style = "color : red"> This is my red paragraph </p>
	</body>
</html>
```

Background : Color , Image , Repeat
we can color the background using 3 methods , naming , RGB , hex code
```css
div{
	background-color : red; /* color name */
	background-color : rgb(red_value  green_value  blue_value / transparensy_ratio %);
	background-color : rgb(255  0  0 / 100 %); /* strong red */
	background-color : rgb(255  0  0 / 0 %); /* very weak red */
	background-color : #FF0000; /* red color , first 2 red , second 2 green , third 2 blue [00 - FF] hex --> [0 , 255] */
	background-image : url("link of the image or path");
	background-repeat : repeat; /* repeat the image as the element width*/
	background-repeat : no-repeat; /* don't repeat neither H or V*/
	background-repeat : repeat-x; /* repeat horzontally (H)*/
	background-repeat : repeat-y; /* repeat vertically (V) */
	background-attachment : fixed ; /* the background move with you in the site*/
	background-attachment : scroll; /* this is the default and the background didn't scroll with you */
	background-position : 20px 30px; /* specify where is the background by directions {center , bottom , top} or by pixels {20 px , 200 px} {from left , from right} or percent {50% 10%} */
	background-size : cover; /* control the size of background by default it's value is auto which is the normal size of the image , we can set it to cover to make it fully cover the background , we can use contain which will cover the whole content but it always visiable with it's full size , we can also use precent in width , height*/
}
```

Padding : give space inside
```CSS
div{
	padding : 10px; /* 10 px in all directions */
	padding-Top : 10px;
	padding-right : 20px;	
	padding-bottom : 30px;
	padding-left : 40px;
	padding : 10px 20px 30px 40px /* up right down left */
}
```

margin : give space outside , it can accept negative values
```CSS
div{
	margin : 10px; /* 10 px in all directions */
	margin-Top : 10px;
	margin-right : 20px;	
	margin-bottom : 30px;
	margin-left : 40px;
	margin : 10px 20px 30px 40px; /* up right down left */
	width = 70%;
	margin-left : 15%;
	margin-left : auto;
	margin-right : auto; /* it distrubte the 100% - width% .5 to right , .5 to left */
}
```

Border : element edges
![[Pasted image 20250325213354.png]]

```CSS
div{
	border-width: 2px 3px 4px 3px;
	border-top-color : blue;
	border-color: red;
	border-style: solid;
	border : 10px solid red; /* short hand border : width | style | color */
}
```

outline : just like border but outside the element while the border is part from the element , note you can't control outline in one direction while the other is not , you apply it to the 4 directions
```CSS
div{
	outline : 10px solid red;
}
```

display property

|Property|`block`|`inline`|`inline-block`|
|---|---|---|---|
|**Default Behavior**|Starts on a new line and takes full width|Stays in the same line and takes only required width|Stays in the same line but allows setting width/height|
|**Width & Height**|Can set `width` and `height`|Cannot set `width` and `height` (respects content size)|Can set `width` and `height`|
|**Margin & Padding**|Affects layout normally|Only horizontal margins/paddings affect layout|Affects layout normally|
|**Line Breaks**|Forces a new line before and after|No line breaks, flows with text|No line breaks, flows with text|
|**Examples**|`<div>`, `<p>`, `<h1>` to `<h6>`, `<section>`|`<span>`, `<a>`, `<strong>`, `<em>`|`<button>`, `<img>`, `<input>`|
|**Best Use Cases**|Structural layout elements|Wrapping text or inline elements|Elements that need inline positioning but with customizable dimensions|
```CSS
span{
	display : block;
}
```


Element visibility 
display none erase the element , while the visibility hidden erase the element while reserve it's location on the screen 
```CSS
div{
	display : none ; /* means element doesn't exist */
	visibility : hidden ; /* by default it's value is visible */
}
```


Grouping multiple selectors
```CSS
.selector1 , .selector2 , .selector3{
	/* you put here the common properties */
}
.selector1 {
	/* what make selector1 special */
}
.selector2{
	/* what make selector2 special */
}
.selector3{
	/* what make selector3 special */
}
```


Nesting
if you want to style like all paragraphs inside a div we can do this
```CSS
div p{
	color : red;
}
div .special{
	color : blue;
}
```


Width 
```CSS
div{
	display : inline-block;
	min-width : 400px; /* at least take width of 400 */
	max-width : 500px;  /* at most take width of 500 */
	min-height : 400px;
	max-height : 500px;
	width : fit-content; /* make the width fit the text however it's width*/
}
```


overflow 
```CSS
div{
	width : 400px;
	height : 400px;
	overflow : visible; /* the text outside the element will be visible even if it is overflowing */
	overflow : hidden; /* force hide the overflow content */
	overflow : scroll; /* make the overflow content visible by scrolling the bar */
	overflow : auto ; /* if the content overflow it add scroll , otherwise nothing */
	overflow-y : hidden; /* hide the overflow in height */
	overflow-x : hidden; /* hide the overflow in width */
}
```


Text color
```CSS
div{
	color : red; /* text color is red now */
	text-shadow : 0 0 0 red/* make shadow to the text (not used much) it accepts H-Shadow(positive or negative) V-Shadow (p or n) Blur color */
}
```

Text Alignment 
```CSS
div{
	text-align : center; /* align the text in the center, it can have value of left or right */
	direction : ltr; /* change page direction , it can rtl*/
	vertical-align : top; /* text up and image down , we can make the value bottom , middle*/
}
```


Text Decoration and Transformation
```CSS
div{
	text-decoration : line-through; /* put line throw the text , to can take value uderline , overline , none */
	text-transform : capitalize; /* capitalize : first letter is captial , uppercase : all the text is capital */
}
```


Spacing and indentation 
```CSS
div{
	letter-spacing : 10px ; /* it can take positive or negative value that define the space between each 2 consecutive letters */
	text-indent : 100px; /* indent the text with certain pixels */
	line-height : 1; /* when you increase the space between lines increase */
	word-spacing : 10px; /* same as letter-spacing but between words */
	white-space : nowrap; /* don't wrap the element when the width end*/
	word-break : break-word; /* break the overflow part to new line */
}
```

text overflow
```CSS
div{
	text-overflow : ellipsis; /* put three dots to show the text is trimed */
}
```

--------
##### Inheritance 
Inheritance in CSS refers to how certain properties of an element are automatically passed down to its child elements. However, not all properties are inherited by default.

Which Properties Are Inherited?

By default, **text-related properties** are inherited:  
✅ `color`  
✅ `font` (including `font-family`, `font-size`, etc.)  
✅ `letter-spacing`  
✅ `visibility`  
✅ `line-height`

```CSS
body {
  color: blue;
  font-family: Arial, sans-serif;
}

p {
  /* Inherits color and font-family from body */
}
```

Which Properties Are NOT Inherited?
Most **box-model** and **layout-related properties** are **not** inherited by default:  
❌ `margin`, `padding`, `width`, `height`  
❌ `border`, `background`, `box-shadow`  
❌ `display`, `position`, `z-index`

```CSS
body {
  background-color: black;
}

p {
  /* Does NOT inherit background-color */
}
```

Forcing Inheritance (`inherit`, `initial`, `unset`)

CSS provides values to **control inheritance** explicitly:

|Keyword|Description|
|---|---|
|`inherit`|Forces the property to be inherited from the parent|
|`initial`|Resets the property to its default value|
|`unset`|Acts as `inherit` for inheritable properties, and `initial` for non-inheritable properties|
```CSS
div {
  color: red;
}

p {
  color: inherit; /* Takes color from div */
}

span {
  color: initial; /* Resets to browser default (usually black) */
}
```

Global Inheritance Trick
If you want **all properties** to inherit by default, you can use:
```CSS
* {
  all: inherit;
}
```

or reset all styles with:
```CSS
* {
  all: unset;
}
```

----
font
```CSS
div{
	font-family : Arial , Helvetica; /* write all families you want the browswer select first available */
	
}
```

Font size and CSS units
```CSS
div{
	font-size : 20px;
	/*
	px
	em ==> 16px this is wrong it is equal to inherated font size Default font size for web pages is 16px
	
	rem ==> Root Time (takes it's size from the root element)
	vw ==> view port width 1vw --> 1% the width of the secreen
	percentage ==> take percentage from parent
	*/
}
```

font style , font variant , font weight
```CSS
div{
	font-style : italic;
	font-style : normal;
	font-variant : small-caps;/* convert to captial letters */
	font-weight : bold; /* convert to bold by any degree 100 - 900 , bold */
	font-weight : normal;
}
```

------
Mouse cursor

```CSS
button{
	cursor : pointer;/* change the mouse pointer to seems like it is clickable */
	cursor : grab; /* change the cursor also */
	cursor : move;
}
```

----
Float , Clear
use in layout to distribute elements 
```CSS
div{
width : 25% ;
float : left; /* left or right */
}
.clear{
	clear : left; /* both or left or right */
}
```

```CSS
div{
	float : left;
	width : calc(88% / 5);
	margin-left : 2%;
	width : calc((100% - 90px) / 5);
}
```

-------
Opacity
it apply transparency to the whole content not the background only
```CSS
.one{
	background-color : red;
	opacity : 0.1 ; /* 1 --> full , 0--> disappear*/
}
```
-----------------
### Position 
The CSS position property defines how an element is positioned in the document. It can control how an element is laid out relative to its normal position or relative to its parent container. Here’s a breakdown of the most common values:

### 1. **Static**

- **Default Behavior:** All elements are positioned as they appear in the normal document flow.
    
- **Characteristics:** They are not affected by top, right, bottom, or left properties.
    
- **When to Use:** Use static positioning when you want the element to follow the normal flow without any modifications.
    

### 2. **Relative**

- **Behavior:** The element is positioned relative to its normal position.
    
- **Characteristics:** You can move it with top, right, bottom, and left properties, but the space for the element is still reserved as if it were in the normal flow.
    
- **When to Use:** When you need to nudge an element slightly without affecting the layout of other elements.
    

### 3. **Absolute**

- **Behavior:** The element is removed from the normal document flow and positioned relative to its closest positioned ancestor (an ancestor with a position other than static).
    
- **Characteristics:** It won’t leave any space in the document; it can overlap other elements.
    
- **When to Use:** Ideal for placing elements precisely in a layout, such as dropdown menus or modals.
    

### 4. **Fixed**

- **Behavior:** The element is removed from the normal flow and fixed relative to the viewport.
    
- **Characteristics:** It stays in the same position even when the page is scrolled.
    
- **When to Use:** Perfect for sticky headers or floating action buttons that need to remain visible on scroll.
    

### 5. **Sticky**

- **Behavior:** A hybrid of relative and fixed positioning. The element behaves as relative until a certain scroll threshold is met, then it “sticks” in place as if it were fixed.
    
- **Characteristics:** It requires additional properties (like top) to determine the threshold.
    
- **When to Use:** When you want an element to scroll with the page until a certain point, then become fixed—common for table headers or navigational menus.
```CSS
/* Static (default) */
.static-box {
  background: lightblue;
  padding: 20px;
  /* No positioning applied */
}

/* Relative */
.relative-box {
  position: relative;
  top: 10px;
  left: 20px;
  background: lightgreen;
  padding: 20px;
}

/* Absolute */
.absolute-box {
  position: absolute;
  top: 50px;
  right: 30px;
  background: lightcoral;
  padding: 20px;
}

/* Fixed */
.fixed-box {
  position: fixed;
  bottom: 0;
  left: 0;
  background: lightgoldenrodyellow;
  padding: 20px;
}

/* Sticky */
.sticky-box {
  position: sticky;
  top: 0; /* Sticks to the top when scrolled */
  background: lightgray;
  padding: 20px;
}

```

### Summary

- **Static**: Default; follows normal flow.
    
- **Relative**: Adjusts relative to its original position.
    
- **Absolute**: Precisely positions relative to the nearest positioned ancestor.
    
- **Fixed**: Remains in place relative to the viewport.
    
- **Sticky**: Switches between relative and fixed depending on scroll position.
-------
The **z-index** property controls the stacking order of positioned elements (i.e., elements whose position is set to something other than static). Essentially, it determines which elements appear on top of others when they overlap. Here are the key points:

### How It Works

- **Stacking Order:** Elements with a higher z-index value will be rendered in front of those with a lower value.
    
- **Positioned Elements:** The property only applies to elements that are positioned (using values such as `relative`, `absolute`, `fixed`, or `sticky`). For elements with the default `static` positioning, z-index has no effect.
    
- **Stacking Context:** A stacking context is a three-dimensional conceptualization of HTML elements along an imaginary z-axis. It is formed by elements that have a position value other than static and a z-index value. Within this context, elements stack in order based on their z-index. A new stacking context can also be created by other properties like `opacity` (if less than 1), `transform`, `filter`, etc.
    
- **Default Behavior:** If two positioned elements have the same z-index, they will stack based on their order in the HTML markup (with later elements on top).
```CSS
/* A lower z-index value; this box will be behind the other */
.back-box {
  position: relative;
  z-index: 1;
  background: lightblue;
  padding: 20px;
}

/* A higher z-index value; this box will appear on top */
.front-box {
  position: relative;
  z-index: 10;
  background: lightcoral;
  padding: 20px;
}

```

In this example, even if the `.back-box` appears later in the HTML, the `.front-box` with a higher z-index will overlap it.

### Summary

- **Purpose:** Controls which elements overlap others.
    
- **Usage:** Only effective on positioned elements.
    
- **Key Considerations:** Remember that z-index only affects stacking within the same stacking context; a new context can be created by various CSS properties.
----
### Responsive design 
Responsive design is an approach to web design that ensures a website's layout, images, and functionalities adjust smoothly across a wide range of devices—from large desktop monitors to smartphones. This approach prioritizes flexibility and user experience by allowing the content to adapt based on screen size, orientation, and resolution.

### What is Responsive Design?

- **Fluid Layouts:** Rather than relying on fixed dimensions, responsive design uses relative units like percentages, ems, or rems so that elements scale proportionally.
    
- **Media Queries:** CSS media queries allow designers to apply different styles based on device characteristics (e.g., screen width or height). This helps tailor layouts for various devices.
    
- **Flexible Images and Media:** Images and other media are made flexible so they can scale within their containing elements without breaking the layout.
    
- **Mobile-First Approach:** Often, responsive design starts with a design that works well on mobile devices, then progressively enhances for larger screens.
    

### How CSS Flexbox Enhances Responsive Design

Flexbox, or the Flexible Box Layout, is a CSS module designed to create more efficient layouts, particularly for responsive designs. It provides a powerful and simple way to distribute space among items in a container, even when their size is unknown or dynamic.

#### Key Concepts and Properties of Flexbox

- **Flex Container and Flex Items:**  
    By declaring `display: flex` on a parent element, it becomes a flex container, and its direct children become flex items. This setup makes it easier to manage spacing, alignment, and distribution.
    
- **Flex Direction (`flex-direction`):**  
    You can set the main axis to be either horizontal (`row`) or vertical (`column`). This allows the layout to adapt more naturally depending on the screen dimensions.
    
- **Flex Wrap (`flex-wrap`):**  
    Flex items can automatically wrap onto multiple lines when there isn’t enough space in one line. This is particularly useful for responsive designs, as it prevents content from being squished into a single line on smaller screens.
    
- **Justify Content (`justify-content`):**  
    This property helps align items along the main axis. You can center them, space them evenly, or push them to the start or end of the container.
    
- **Align Items and Align Content (`align-items`, `align-content`):**  
    These properties control alignment along the cross-axis. They ensure that items are appropriately aligned vertically (or horizontally, if the flex direction is column).
    
- **Order, Grow, and Shrink:**
    
    - The **order** property lets you rearrange the visual order of flex items without changing the HTML structure.
        
    - The **flex-grow** and **flex-shrink** properties determine how items expand or contract relative to each other, providing a dynamic and adaptable layout.
        

#### Enhancing Responsive Design with Flexbox

- **Dynamic Adjustments:**  
    With flexbox, the container automatically adjusts to the available space. For example, on larger screens, items might appear side by side, while on smaller devices, they can stack vertically.
    
- **Simplified Layout Management:**  
    Traditional methods like floating elements can be cumbersome and require additional clearfix hacks. Flexbox offers a more intuitive way to control spacing, alignment, and reordering, making it easier to implement responsive behaviors.
    
- **Efficient Space Distribution:**  
    Flexbox's ability to distribute space among items ensures that layouts remain balanced and visually appealing, regardless of screen size. It handles both the alignment and spacing without needing complex calculations.
    
- **Responsive Navigation Menus:**  
    Navigation menus are a common challenge in responsive design. Using flexbox, designers can create menus that switch from horizontal lists on desktops to vertical stacks on mobile devices, often with just a few lines of CSS.
    
Responsive design and CSS Flexbox work hand in hand to create web layouts that are both aesthetically pleasing and functionally robust across different devices. While responsive design provides the overall strategy for flexible and adaptable layouts, flexbox offers a practical and efficient way to manage the alignment, distribution, and order of elements within those layouts. Together, they help ensure that users enjoy a consistent and optimal experience no matter how they access a website.

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Responsive Flexbox Example</title>
  <style>
    /* Basic styling for the container and items */
    .flex-container {
      display: flex;
      flex-direction: row; /* Default: horizontal layout */
      justify-content: space-around;
      align-items: center;
      background-color: #f0f0f0;
      padding: 20px;
      gap: 10px;
    }
    
    .flex-item {
      background-color: #4CAF50;
      color: white;
      padding: 20px;
      text-align: center;
      flex: 1; /* Allows items to grow equally */
      border-radius: 5px;
    }
    
    /* Responsive adjustment: stack items vertically on screens smaller than 600px */
    @media (max-width: 600px) {
      .flex-container {
        flex-direction: column;
      }
    }
  </style>
</head>
<body>
  <div class="flex-container">
    <div class="flex-item">Item 1</div>
    <div class="flex-item">Item 2</div>
    <div class="flex-item">Item 3</div>
  </div>
</body>
</html>
```

### Grid
CSS Grid is a powerful layout system that allows you to create two-dimensional grid-based designs with rows and columns. It provides a structured way to control layout and spacing, making it easier to design complex layouts without relying on floats or positioning hacks. Unlike Flexbox—which is primarily a one-dimensional layout tool (either row or column)—Grid lets you manage both dimensions simultaneously.

### Key Features of CSS Grid

- **Two-Dimensional Layout:** Manage both rows and columns to create a complete grid.
    
- **Explicit and Implicit Grids:** You can define grid lines explicitly, or let the browser auto-create rows and columns as needed.
    
- **Grid Areas:** Define named areas within the grid for more semantic and maintainable layouts.
    
- **Alignment and Spacing:** Use properties like `justify-items`, `align-items`, `grid-gap` (or `gap`), and others to control the alignment and spacing of grid items.
```HTML
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Responsive CSS Grid Example</title>
  <style>
    /* Basic styling for the grid container and items */
    .grid-container {
      display: grid;
      grid-template-columns: repeat(3, 1fr); /* Three equal columns */
      gap: 20px;
      padding: 20px;
      background-color: #f9f9f9;
    }

    .grid-item {
      background-color: #2196F3;
      color: white;
      padding: 20px;
      text-align: center;
      border-radius: 5px;
    }

    /* Responsive adjustment: one column on screens smaller than 600px */
    @media (max-width: 600px) {
      .grid-container {
        grid-template-columns: 1fr;
      }
    }
  </style>
</head>
<body>
  <div class="grid-container">
    <div class="grid-item">Item 1</div>
    <div class="grid-item">Item 2</div>
    <div class="grid-item">Item 3</div>
    <div class="grid-item">Item 4</div>
    <div class="grid-item">Item 5</div>
    <div class="grid-item">Item 6</div>
  </div>
</body>
</html>
```

### Media queries
Media queries are a CSS feature that enables you to apply styles based on specific conditions, such as viewport size, device orientation, resolution, or even specific features of the device. This allows you to create tailored designs that adjust seamlessly across a range of devices, which is a key component of responsive web design.

### How Media Queries Work

- **Conditional CSS Rules:**  
    Media queries let you define CSS rules that only apply when certain conditions are met. For example, you might only want certain styles to load when the screen width is less than 600px.
    
- **Breakpoints:**  
    The conditions often involve breakpoints, which are specific screen widths or other criteria. When the viewport reaches these breakpoints, the specified styles are applied.
    
- **Targeting Devices:**  
    You can also use media queries to target device features, such as screen orientation (landscape or portrait), resolution, or even print styles.
```pesudocode
@media (condition1) and (condition2) and .... {
	the style will apply if the condition is true
}
```

Example using CSS
```CSS
/* Default styles for larger screens */
body {
  font-size: 18px;
  background-color: #fff;
}

/* Styles for screens 600px wide or smaller */
@media (max-width: 600px) {
  body {
    font-size: 16px;
    background-color: #f0f0f0;
  }
}

```
we can also specify the condition in linking the sheet
```Html
<link rel = "stylesheet" href = "Css/print.Css" media = "(max-width: 600px)" />
<style media = "(max-width: 600px)" >
	/* CSS code */
</style>
```

usage
```HTML
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Media Queries Example</title>
  <style>
    /* Default layout for larger screens */
    .container {
      display: flex;
      flex-direction: row;
      justify-content: space-between;
      padding: 20px;
      background-color: #e0e0e0;
    }
    .box {
      flex: 1;
      margin: 10px;
      padding: 20px;
      background-color: #4CAF50;
      color: white;
      text-align: center;
      border-radius: 5px;
    }

    /* Responsive layout for screens 600px wide or smaller */
    @media (max-width: 600px) {
      .container {
        flex-direction: column;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="box">Box 1</div>
    <div class="box">Box 2</div>
    <div class="box">Box 3</div>
  </div>
</body>
</html>
```

### Boot Strap framework
Bootstrap is a popular front-end framework that simplifies web development by offering a collection of pre-styled CSS and JavaScript components, a responsive grid system, and utility classes. It helps developers build modern, mobile-first websites quickly without starting from scratch.

### Key Features of Bootstrap

- **Responsive Grid System:**  
    Bootstrap’s grid system is built on Flexbox and divides the page into 12 columns, making it easy to design responsive layouts that adapt to various screen sizes.
    
- **Pre-Styled Components:**  
    The framework provides a wide range of ready-to-use components such as navigation bars, modals, forms, buttons, cards, and more, which can be easily customized to fit your project’s style.
    
- **Utility Classes:**  
    Bootstrap includes a comprehensive set of utility classes for quickly managing margins, padding, colors, typography, and other styling properties without writing custom CSS.
    
- **Mobile-First Approach:**  
    The framework is designed with a mobile-first philosophy, ensuring that websites are optimized for smaller screens before scaling up to larger displays.
    

### Example: Using Bootstrap for a Responsive Layout

Below is a simple example that demonstrates how to create a responsive layout using Bootstrap’s grid system and components.
```HTML
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Bootstrap Responsive Example</title>
  <!-- Bootstrap CSS (using CDN for quick setup) -->
  <link
    href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css"
    rel="stylesheet"
    integrity="sha384-ENjdO4Dr2bkBIFxQpeoZwB7B7sZ4bs+6v5p3t7P5D6jzKXrNQ6rwz1MZCdI6xXGZ"
    crossorigin="anonymous">
</head>
<body>
  <!-- Navigation Bar -->
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
    <div class="container-fluid">
      <a class="navbar-brand" href="#">MySite</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" 
              aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav ms-auto">
          <li class="nav-item">
            <a class="nav-link active" aria-current="page" href="#">Home</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">Features</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">Pricing</a>
          </li>
        </ul>
      </div>
    </div>
  </nav>

  <!-- Responsive Grid Layout -->
  <div class="container my-5">
    <div class="row">
      <!-- Each column will stack on small screens and sit side by side on larger screens -->
      <div class="col-md-4 mb-4">
        <div class="card">
          <div class="card-body">
            <h5 class="card-title">Card 1</h5>
            <p class="card-text">This is an example card using Bootstrap.</p>
          </div>
        </div>
      </div>
      <div class="col-md-4 mb-4">
        <div class="card">
          <div class="card-body">
            <h5 class="card-title">Card 2</h5>
            <p class="card-text">Cards adjust responsively with Bootstrap's grid system.</p>
          </div>
        </div>
      </div>
      <div class="col-md-4 mb-4">
        <div class="card">
          <div class="card-body">
            <h5 class="card-title">Card 3</h5>
            <p class="card-text">On small devices, these cards stack vertically.</p>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- Bootstrap JavaScript Bundle (includes Popper) -->
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js" 
          integrity="sha384-kenU1KFdBIe4zVF0s0G1M5b4hcpxyD9F7jL+pbZ9l+Yx3sIlD7u9g4Mgm0L5Vx04" 
          crossorigin="anonymous"></script>
</body>
</html>
```