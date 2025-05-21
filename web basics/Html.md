HTML : Hypertext Markup Language , the language to build the structure of the web page

elements : consists of opening tag , closed tag (some doesn't have it)
```html
<html> </html> <!-- root element >
```

Each html page consists of 2 important parts
- head : contain meta data
- body : the page content

head can have a tag title
```html
<html>
	<head>
		<title> X </title>
	</head>
	<body>
		content
	</body>
</html>
```

inside head we can use meta tag (self closing tag)
meta can have attributes like charset , name , content , ....
```html
<meta charset = "UTF-8" />
<meta name = "description" content = "this is my website" />
```

style tag , we can write in it CSS code
```html
<html>
	<head>
		<style>
			body{
				background-color : red;
			}
		</style>
	</head>
</html>
```

script tag
```html
<html>
	<head>
		<script> </script>
	</head>
</html>
```

link tag
- rel : the relationship of this file with your page
- href: where the file is
```html
<html>
	<head>
		<link rel = "stylesheet" href = ""/>
	</head>
</html>
```

comments
```html
<!-- single line comment -->
<!--
	multiple line comment
-->
```

Doc type : it is instruction we give for the browser to know the html version we work with
```html
<!doctype html> <!-- render in standard mode not Quirks -->
```

##### Headings
in heading we have six levels h1-h2-....-h6 


browser ignore extra spaces , lines
when giving attributes values we can put them in single / double quotes or mix of them or if it was one word then we can ignore quotes at all

##### Paragraph Element
```html
<p> </p>
```

paragraph element is blocking element takes one line and empty line before and after , nothing beside it in the same line , inline elements are in the same line

##### Element Attributes
- Global attributes: attributes can be applied to any elements , ex: class , hidden
- element attributes: specially applied to the element used it , img {src , alt} , a{href} , audio{src} , video{src}

##### Formatting elements
- b (just text) / strong (important text) tag convert text to bold
- i / em make the text italic , same difference as b , strong
- mark : mark the text with highlight
- u : underline 
- small : smaller text (smaller than the content surrounding it)
- del : deleted text
- ins : inserted text
- sub: Subscript a_i
- sup: superscript a ^ i

```html
<b>bold text</b>
<strong> strong bold text </strong>
<i> italic text </i>
<em> italic text </em>
<mark> marked text </mark>
<u> underlined text </u>
<del> deleted text </del>
<ins> inserted text </ins> 
a <sub> i </sub>
a <sup> i </sup>
```

Anchor Tag (inline) , by clicking on text it redirect you to the link , if you let it by default then it will open in your window , but if we want to open new page use target = "blank" , we can add title which will appear if you moved the mouse to the link and leave it

```html
<a href = "link you go to"> text redirect you </a>
<a href = "link you go to" target = "_blank"> text redirect you </a>
<a href = "link you go to" tilte = "google"> Go to google </a>
```

Anchor tag can redirect you to location in same page
```html
<a href = "#label" > go to your label </a>
<p id = "label" > hello mother fucker </p>
```

Anchor tag can redirect you to Email of a person , when you click it , will open your mail app and put in to field the user mail in href
```html
<a href = "mailto:o@gmail.com"> contact me</a>
```
-------------
##### images
img tag has 2 attributes {src , alt}
- src : where is the photo
- alt : what text to show as replace if we can't render the image
- width: number of pixel you want to custom the width of the image
- Height : number of pixel you want to custom the height of the image
```html
<img src = "relative path" alt "photo is not available" height = "200px"/>
```

-------
##### List
ul --> unordered list , it just create list of unordered items just using bullet points (not numbered) , we can also make nested unordered lists 
```html
This is my book store
<ul>
	<li> HTML </li>
	<li> CSS  </li>
	<li> JS   </li>
</ul>
```
![[Pasted image 20250319153023.png]]

ol --> ordered list , create list of ordered items and number them using integers and we can make them nested
we can reverse the order using reversed keyword
we can use offset as start by setting start = "x"
we can change type of ordering by setting type = "type order"
```html
<ol>
	<li> HTML </li>
	<li> CSS  </li>
	<li> JS   </li>
</ol>
<ol reversed>
	<li> HTML </li>
	<li> CSS  </li>
	<li> JS   </li>
</ol>
<ol start = "100">
	<li> HTML </li>
	<li> CSS  </li>
	<li> JS   </li>
</ol>
<!-- same as above -->
<ol>
	<li value = "100" > HTML </li>
	<li> CSS  </li>
	<li> JS   </li>
</ol>
<ol type = "A">
	<li> HTML </li>
	<li> CSS  </li>
	<li> JS   </li>
</ol>
```

dl --> description list
dt -->  term
dd --> description term
```html
<dl>
	<dt> HTML </dt>
	<dd> Language of the web structure </dd>
	<dd> The First Lan </dd>
</dl>
```

![[Pasted image 20250319154252.png]]

----

##### Table
good way to represent data , it consists of 3 main tags thead for the header , tbody for the body , tfoot for the footer
- thead represent the column name
- tbody represent the data itself
- tr --> table row
- td --> table data cell
- th --> table header cell , it is bold
rearrange the order like get the footer before the header it not effect the table structure
we can use some attributes
- border = "value" --> it creates edges around the table
- cellpadding = "value px" --> padding between cells
To merge cells , or to make cell take certain width of cells we can use colspan attribute
```html
<table border = "1">
	<caption>
		title of the table it will show above the table in the middle
	</caption>
	<thead> 
		<tr>
			<th> first column </th>
			<th> second column </th>
			<th> third column </th>
		</tr>
    </thead>
    <tbody>
	    <tr>
		    <td> first data cell in first row </td>
		    <td> first data cell in first row </td>
		    <td> first data cell in first row </td>
		</tr>
	</tbody>
	<tfoot>
		<tr>
			<td colspan = "3"> footer </td>
		</tr>
	</tfoot>
</table>
```


Span tag , it is inline element has no style by user agent (browser)
we can use it to allow different styles to words inside paragraph , like style word My in the sentence "this is My library"
```html
<span> word without style in the borwser </span>
```

br tag : it just make new line
```html
<br/>
```

hr tag : it just make horizontal line
```html
<hr/>
```


##### div element
div is a container to the other elements , make styling group of elements easier
```html
<div> 
	<h1> hello </h1>
	<p> hello </p>
</div>
```
---------------------
##### HTML Entities
&lt; ---> <
&gt; --> >
&copy; --> copy right

we can google any special character code

##### Semantic elements
Header --> Head of website
nav --> navigation bar
section --> website body
figure --> put image in it
```html
<Header>
	Head of the webiste
</Header> 
<nav>
	Link - Link ...
</nav>
<section>
	<figure>
		<img src = "link" alt = "not found">
		<figcaption> caption of the image </figcaption>
	<section> </section>
	<aside> </aside>
</section>
```

![[Pasted image 20250319234431.png]]
##### Layout using DIV and classes

```html
<html>
    <head>
        <meta charset = "UTF-8" />
        <title> Book Store </title>
        <meta name = "description" content = "this is our book store"/>
    </head>

    <body>

        <div class = "header">
            <h2> logo </h2>
            <ul>    
                <li> home </li>
                <li> about </li>
                <li> services </li>
                <li> contact us </li>
            </ul>
        </div>

        <hr/>

        <div class = "navigation">
            <ul>
                <li> link </li>
                <li> link </li>
                <li> link </li>
                <li> link </li>
            </ul>
        </div>

        <hr/>

        <div class = "content">
            content
        </div>
        
        <div class = "sidebar">
            this is sidebar
        </div>
        
        <hr>
        <div class = "footer">
            Footer
        </div>
    </body>
</html>
```

##### Layout using Semantic Elements
```html
<html>
    <head>
        <meta charset = "UTF-8" />
        <title> Book Store </title>
        <meta name = "description" content = "this is our book store"/>
    </head>

    <body>
        <header>
            <h2> logo </h2>
            <ul>    
                <li> home </li>
                <li> about </li>
                <li> services </li>
                <li> contact us </li>
            </ul>
        </header>

        <hr/>
        <nav>
            <ul>
                <li> link </li>
                <li> link </li>
                <li> link </li>
                <li> link </li>
            </ul>
        </nav>

        <hr/>

        <section>
            content
        </section>

        <aside>
            this is sidebar
        </aside>

        <hr>

        <footer>
            Footer
        </footer>

    </body>

</html>
```

---------------
##### Audio tag
when we use audio tag we can add src attribute and give the path or we can use source element , the benefits that we can use more than audio file  and the browser choose the one that fits with it.

we can also add controls attribute , which allow you to control the audio (play , stop , ...)
we can also add auto play attribute which will auto play the audio when the site open
we can also add loop attribute which will re open the audio when it finishes
we can also add muted attribute which will mute the audio by default and the use can reopen it.

```html
<audio src = "path" controls autoplay loop muted>
	<source src = "path" type = "MIME TYPE , eg: for mp3 : audio/mpeg">
	<source src = "path">
	Your browser does not support audio tag <!-- you see this message if not source can be opened -->
</audio>
```

![[Pasted image 20250321134936.png]]

-----
##### Video tag
same as Audio but represent a video

poster : attribute you provide the alternative picture that shows until the video loads

track : element you add when you want to add translation or any thing extra to the video

vtt: video text track
```html
<video src = "path" controls width = "600" height = "400" autoplay loop muted poster = "pic path showed until the video loads">
	<source src = "option 1" type = "video/mp4">
	<source src = "option 2" type = "video/ogg">
	Your browser Does not support video tag
	<track src = "my_file_en.vtt" kind = "type of the file" srclang = "en" label = "English">
	<track src = "my_file_it.vtt" kind = "type of the file" srclang = "it" label = "Italian">
</video>
```

-------
## HTML Form 

An HTML form is used to collect user input and submit that information to a server or another page for processing (e.g., for logging in, searching, or other data submission tasks). The form itself is created with the `<form>` tag, which may include attributes such as:

- **action:** Specifies the URL or path where the form data will be sent. If omitted or empty, the form submits to the same page.
    
- **method:** Determines which HTTP method to use:
    
    - **GET:** Appends form data to the URL query string.
        
    - **POST:** Sends form data as a hidden payload.
        
- **novalidate:** When added to the form, it disables built-in HTML5 form validation.
    
- **target:** Defines where to display the response (e.g., `target="_blank"` opens the result in a new tab).
    

---

## Input Element and Its Attributes

The `<input>` tag is one of the most common form elements. It is an inline, self-closing element that requires a `type` attribute to define its behavior. Other common attributes include:

- **name:** The key used when submitting data as a key-value pair.
    
- **value:** Sets a default value for the input. This text appears as the current input which the user can change.
    
- **placeholder:** Provides a hint or description inside the input when it’s empty.
    
- **required:** Indicates that the field must be filled out before submitting the form.
    
- **readonly:** Makes the input non-editable; its value will be submitted.
    
- **disabled:** Prevents user interaction and does not include the field’s value in the submitted data.
    
- **autofocus:** Automatically focuses this element when the page loads.
    
- **minlength / maxlength:** Sets a minimum or maximum number of characters for the input.
    

---

## Specific Input Types

### 1. **Button and Control Types**

- **submit:** Creates a submit button that sends the form data when clicked.
    
- **reset:** Creates a button that resets all form fields to their initial values.
    
    - _Example:_ `<input type="reset" value="Reset">`
        
- **hidden:** Stores data that the user cannot see or modify.
    
- **color:** Displays a color picker and sends the chosen color’s hex code.
    
- **range:** Creates a slider control.
    
    - **Attributes:**
        
        - `min`: The minimum value.
            
        - `max`: The maximum value.
            
        - `step`: The increments by which the slider moves.
            
        - `value`: The initial value.
            
- **number:** Restricts the input to numerical values.
    
    - **Attributes:**
        
        - `min`: The minimum numeric value.
            
        - `max`: The maximum numeric value.
            
        - `step`: Increments for increasing or decreasing the number.
            
- **search:** Creates a text field optimized for search queries.
    
- **url:** Validates that the input is a URL.
    
- **date, month, time:** Allow users to choose a date, month, or time via a calendar or spinner interface.
    
- **file:** Enables the user to select a file from their device.
    

### 2. **Selection Types**

- **radio:** Creates a circular button for selecting one option from a group.
    
    - **Usage Tips:**
        
        - Use the same `name` for a group of radio buttons so that only one can be selected.
            
        - The `checked` attribute can be used to set a default selection.
            
        - For better accessibility, pair with a `<label>` and use a unique `id` for each radio button.
            
- **checkbox:** Creates a square box for selecting one or more options.
    
    - **Usage Tips:**
        
        - Use the same `name` if you want the selections to be grouped.
            
        - Multiple checkboxes allow for multiple selections.
            

---

## Other Form Elements

### 1. **Label**

- The `<label>` element is used to describe an input. It is beneficial for accessibility as clicking the label focuses or toggles the corresponding input.
    
- You can associate a label with an input by using the `for` attribute, which should match the input’s unique `id`.
    

### 2. **Select and Option**

- The `<select>` element creates a drop-down list.
    
    - **Attributes:**
        
        - `name`: Used as the key for the selected value.
            
        - `id`: For connecting with a `<label>`.
            
        - `multiple`: Allows the user to select more than one option.
            
- **Options:**
    
    - Each `<option>` element within `<select>` defines an item in the list.
        
    - The `selected` attribute can be used to set the default selected option.
        
    - `<optgroup>` can be used to group related options together.
        

### 3. **Textarea**

- The `<textarea>` element creates a multi-line text input.
    
- **Attributes:**
    
    - `placeholder`, `maxlength`, and `minlength` can be used similarly to `<input>` fields.
        
    - The element is resizable by the user unless CSS styling limits it.
        

### 4. **Input List with Datalist**

- An `<input>` can be paired with a `<datalist>` element to provide a list of predefined options that the user can choose from while still allowing free-form input.
    
    - **Example:**
        
        - `<input list="programming" name="prog">`
            
        - `<datalist id="programming">` contains several `<option>` elements.


##### Complete HTML Code Example
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>HTML Form Example</title>
</head>
<body>
  <h1>HTML Form Example</h1>
  <form action="/submit-form" method="POST" target="_blank" novalidate>
    <!-- Text Input with Placeholder, Required, and Character Limits -->
    <div>
      <label for="username">Username:</label>
      <input type="text" id="username" name="username" placeholder="Enter your username" required minlength="4" maxlength="12">
    </div>

    <!-- Password Input -->
    <div>
      <label for="password">Password:</label>
      <input type="password" id="password" name="password" required>
    </div>

    <!-- Email Input -->
    <div>
      <label for="email">Email:</label>
      <input type="email" id="email" name="email" placeholder="example@domain.com">
    </div>

    <!-- Color Picker -->
    <div>
      <label for="favcolor">Favorite Color:</label>
      <input type="color" id="favcolor" name="favcolor" value="#ff0000">
    </div>

    <!-- Number Input -->
    <div>
      <label for="age">Age (18-100):</label>
      <input type="number" id="age" name="age" min="18" max="100" step="1" value="30">
    </div>

    <!-- Range Slider -->
    <div>
      <label for="volume">Volume:</label>
      <input type="range" id="volume" name="volume" min="0" max="100" step="10" value="50">
    </div>

    <!-- Radio Buttons -->
    <div>
      <p>Gender:</p>
      <input type="radio" id="male" name="gender" value="male" checked>
      <label for="male">Male</label>
      <input type="radio" id="female" name="gender" value="female">
      <label for="female">Female</label>
      <input type="radio" id="other" name="gender" value="other">
      <label for="other">Other</label>
    </div>

    <!-- Checkboxes -->
    <div>
      <p>Select your hobbies:</p>
      <input type="checkbox" id="reading" name="hobbies" value="reading">
      <label for="reading">Reading</label>
      <input type="checkbox" id="sports" name="hobbies" value="sports">
      <label for="sports">Sports</label>
      <input type="checkbox" id="music" name="hobbies" value="music">
      <label for="music">Music</label>
    </div>

    <!-- Select with Option Groups -->
    <div>
      <label for="country">Country:</label>
      <select id="country" name="country">
        <optgroup label="North America">
          <option value="usa" selected>USA</option>
          <option value="canada">Canada</option>
        </optgroup>
        <optgroup label="Europe">
          <option value="uk">United Kingdom</option>
          <option value="germany">Germany</option>
        </optgroup>
      </select>
    </div>

    <!-- Textarea -->
    <div>
      <label for="bio">Biography:</label>
      <textarea id="bio" name="bio" rows="4" placeholder="Tell us about yourself..." maxlength="200"></textarea>
    </div>

    <!-- Input with Datalist -->
    <div>
      <label for="prog">Programming Language:</label>
      <input list="programming" id="prog" name="prog" placeholder="Start typing...">
      <datalist id="programming">
        <option value="JavaScript">
        <option value="Python">
        <option value="Java">
        <option value="C#">
      </datalist>
    </div>

    <!-- File Upload -->
    <div>
      <label for="fileUpload">Upload your file:</label>
      <input type="file" id="fileUpload" name="fileUpload">
    </div>

    <!-- Date Input -->
    <div>
      <label for="date">Select a Date:</label>
      <input type="date" id="date" name="date">
    </div>

    <!-- Month Input -->
    <div>
      <label for="month">Select a Month:</label>
      <input type="month" id="month" name="month">
    </div>

    <!-- Time Input -->
    <div>
      <label for="time">Select a Time:</label>
      <input type="time" id="time" name="time">
    </div>

    <!-- Hidden Input (sends data without user interaction) -->
    <input type="hidden" name="token" value="123456789">

    <!-- Readonly Field -->
    <div>
      <label for="readonlyField">Read-Only Field:</label>
      <input type="text" id="readonlyField" name="readonlyField" value="Cannot change this" readonly>
    </div>

    <!-- Disabled Field (won't be submitted) -->
    <div>
      <label for="disabledField">Disabled Field:</label>
      <input type="text" id="disabledField" name="disabledField" value="Not submitted" disabled>
    </div>

    <!-- Form Buttons -->
    <div>
      <input type="submit" value="Submit">
      <input type="reset" value="Reset">
    </div>
  </form>
</body>
</html>

```

-----------
Q tag it is simple inline tag that surround the text with ""  , we can use block quote to be displayed as block element.

button element , we can use button inside in the form , when we press button it make an action

we can use wbr element to allow breaking text into new line
we can use Bdi to isolate the text from the page direction useful when mix of language like English and Arbic in same website
```html
<q>
	this is quoted text
</q>
<blockquote>
	this is quoted text
</blockquote>
<button> click here! </button>
<div>
	hhhhhhhhhhhhhhhhhhhhhhhhhhh<wbr/>hhhhhhhhhhhhhhhhh<wbr/>hhhhhhhhhhhhhhhhhhhh
</div>
<p> <Bdi>شيتباشتنيسب</Bdi> jkaldsfjlkadfsjlkadsfj
```

code element , we can write code within it
```html
<code>
write your code here
</code> 
```

pre element allow to capture spaces and lines , note we can mix between pre , code to allow better visibility  
```html
<pre>
	this is me 
		mem 
				me
</pre>
```

iframe element we can use it to load another page into our site
```html
<iframe src = "path or link" frameborder = "0" width = "100" height = "100"> </iframe>
```

---
Accessibility : group of rules for the Accessibility of the users 

**Accessibility in HTML** ensures that web content is usable by everyone, including people with disabilities. It follows **WCAG (Web Content Accessibility Guidelines)** and includes:

1. **Semantic HTML** – Using proper tags like `<nav>`, `<header>`, `<article>` for better screen reader support.
    
2. **Alt Text** – Adding `alt` attributes to images for visually impaired users.
    
3. **Keyboard Navigation** – Ensuring elements are accessible using `Tab`, `Enter`, and `Space` keys.
    
4. **ARIA (Accessible Rich Internet Applications)** – Using `aria-label`, `role`, and other attributes to enhance accessibility.
    
5. **Forms & Labels** – Using `<label>` with `<input>` and providing clear instructions.
    
6. **Contrast & Readability** – Ensuring sufficient color contrast and resizable text.

---
ARIA : Accessible rich internet Application
ARIA (Accessible Rich Internet Applications) is a set of attributes that improve web accessibility for people with disabilities. It enhances dynamic content and user interface elements that standard HTML does not support well for screen readers and other assistive technologies. ARIA roles, states, and properties help define how elements should be interpreted, making interactive applications more accessible.

ARIA attributes are categorized into three main types:

1. **Roles** – Define the purpose of an element (e.g., `role="button"`, `role="alert"`, `role="dialog"`).
    
2. **States** – Indicate dynamic changes (e.g., `aria-checked="true"` for a checkbox, `aria-disabled="true"` for a disabled button).
    
3. **Properties** – Provide additional metadata about elements (e.g., `aria-label="Close"`, `aria-labelledby="id"` to reference another element).
    

These attributes help assistive technologies understand and interact with complex web components.