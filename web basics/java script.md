what is java Script?
- scripting or programming language
- working on client side and server side
- implement complex features on web page by making it interactive
	- dynamically update content
	- Manipulating HTML and CSS
	- Animate Images and Content and create image gallery
	- Manipulate and validate Data
	- Control multimedia (Audio , video , ...)
	- web browser games
	- mobile apps

to embed the js code into the HTML we do the following
```html
<script> // js code  </script>
<script src = "path to the file"> </script>
```

comments 
```js
// single line
/* multiple line */
```

Alert
block the content and show message in middle top of the screen
```js
window.alert("hello , world");
```

write
allow to add html code to the page but this is not professional way
```js
document.write("write html code");
```

printing to console
```js
console.log("your message");          // normal print to the console
console.error("your error message"); // print error message
console.table(["osama" , "Ahmed"]); // print as table , index , value
```
-------------
type of operator
in JS we have datatypes
- string
- Number : {float , decimal , integer , long long} all represent with type number
- array : {[]} its type is object
- object { key : value , key : value , key : value ,.......}
- Boolean : {true , false}
- undefined : {not exist}
- null : object has no value 
```js
console.log(typeof "khalwsh");
```
-----------
variables
```js
// allow redeclare , access before declear (undefined) , global scope
var x = 3;
// no redeclare , access before declear (error) , local scope
let y = 2;
// no redeclare , access before declear (error) , local scope
const z = 10;
```
----
Template Literals
```js
let x = "khaled";
console.log(`hello , ${x}`); // hello , khaled
console.log(`hello , 
${x}`); // hello ,
        // khaled
console.log(`hello "${x}"`);
```
----
unary operators
- unary plus / negation (negate it) (return number if its not number)
we can convert strings to numbers using Number constructor
```js
console.log(Number("199"));
```
----
Number
Number is a function in javascript and has some function
```js
Number.MAX_SAFE_INTEGER
Number.MAX_VALUE
```

Number methods
- toString
- toFixed
- parseInt()
- parseFloat()
- isInteger()
- isNan()
----
Math object
- round()
- ceil()
- floor()
- min()
- max()
- pow()
- random()
- trunc() : return the integer part
------
string methods
- Access with index
- access with charAt()
- length
- trim()
- toUppercase()
- toLowercase()
- Chain Methods
- indexOf(value , start)
- lastIndexOf(value , start)
- slice(start , End)
- repeat(Times)
- split(Separator , Limit)
- substring(start , end)
- substr(start , characters to extract)
---
Comparison operators
Equal
- == compare value only ignoring data type
- === deep Equal , compare value and data type
----
if conditions
```js
if (condition){}
else if(condition){}
else {}
```
ternary operator
```js
let y = 2;
let x = (y === 3 ? 2 : 1);
```
Nullish coalescing operator
```js
let price = null;
console.log(`The price is ${price ?? 200}`); // if price is null return 200
```
switch statement
```js
switch(expression){
	Case 1 : 
		// code block
		break;
	Case 2
		// code
		break;
	Default : 
		// code
}
```
----
Arrays
```js
let myFriends = ["ahmed" , "khalwsh" , "sersawy"];
console.log(Array.isArray(myFriends));
```

```js
let myFriends = ["ahmed" , "khalwsh" , "sersawy"];
myFriends[3] = "x";
myFriends[6] = "y";
// myfriends[4] , myfriends[5] : is empty un defiened 

myFriends.unshift("aldksjf" ,"alkd;j" , "akjdlfj"); // add those in the begining of the array
myFriends.push("aldksjf" ,"alkd;j" , "akjdlfj"); // add those in the end of the array
let first = myFriends.shift(); // erase and return array[0]
let last  = myFriends.pop();   // remove last element and return it
```

```js
let arr = [3 , 234 , 3456 , 234 ,3 456, 1234];
arr.indexOf(Search element , from index); // return index of element
arr.lastIndexOf(search element , from index); // last index
arr.includes(value to find , from Index); // return true or false
arr.sort(); // inplace
arr.slice(start , end); // end exculsive
arr.splice(insert index , delete how many element , elements ...);
```

```js
let arr1 = [234 ,234 , 234 , 45];
let arr2 = [234 , 3456 , 234];
let arr3 = [345];
let all = arr1.concat(arr2 , arr3 , "item" , ["items"]);
let string = all.join(seprator); // generate string of element sperate by seprator
```
----
```js
for(let i = 0;i < 10;i++) console.log(i);
```

```js
main:for(let i = 0;i < n;i++){
	for(let j = 0;j < n;j++){
		break main;
	}
}
int n = 1;
while(n < 10)n++;
do{
	n++;
}while(n < 20);
```
------
Functions
```js
function sum(a = 0 , b = 0){
	return a + b;
}
function sum(...numbers){ // ... must be last parameter
	let res = 0;
	for (let i = 0;i < numbers.length;i++){
		res += numbers[i];
	}
	return res;
}
let addition = function(a , b){
	return a + b;
};

let value = () => {
	return 10;
}
// in case of one statement
let value = () => 10;
let value = _ => 10;
let value = (num) => num;
let value = num => num; // in case of one parameter the () can be deleted
let value = (num) => {
	num++;
	return num;
}
```
---
Higher order functions
it is a function that accepts functions as parameters and/or returns a function

types 
- Map
	- method creates a new array
	- populated with the results of calling a provided function on every element
	- in the calling array
	- syntax : map(callbackfunction(element , index , array) {} , thisArg)
		- element : the current element being processed in the array
		- Index : the index of the current element being processed
		- array : the current array
	- note : it return a new array
```js
let nums = [1 , 2 , 3 , 4 , 5 , 6 , 7];
// return array [2 , 4 , 6 , 8 , 10 , 12 , 14];
// element is mandatory , index , arr optional 
nums = nums.map(function(element , index , arr){
	return 2 * element;
});
```

- Filter
	- method creates new array
	- with all element that pass the condition (predicate user send)

```js
let numbers = [11 , 20 , 30 , 40];
numbers = numbers.filter(function(element){
	return element % 2 == 1;
});
```

- Reduce
	- method executes a reducer function on each element of the array
	- resulting in a single output value

```js
let numbers = [10 , 20 , 30];
// let value = numbers.reduce(function(accumlator , element , index , arr){
// } , initial value);
let value = numbers.reduce(function(accumlator , element , index , arr){
	return acc + current;
});
console.log(value); // 60
```

- forEach
	- method executes a provided function once for each array element

```js
let numbers = [2 , 324 , 54 , 65, 23 , 546];
numbers.forEach(function(element){
	// do some logic
});
```
----------
Objects
```js
let user = {
	// properties
	theName : "khaled" , 
	age : 30 , 
	"country of" : "Egypt", // I put "" to make the property name as string
	// Methods
	sayHello : function() {
		return "Hello";
	},
};
console.log(user.sayHello());
console.log(user["country of"]); // can't user."country of"
let x = "country of";
console.log(user[x]); // can't user."country of"
let user2 = new Object();
let user3 = new Object({
	age : 30,
});
```

this key word : it refer to the owner of the method

creating objects out of existing object
```js
let user = {
	age : 30
}
let obj = Object.create(user);
let finalobject = Object.assign(target , objects);
```

----
DOM : Document object Model
```js
let myIdElement = document.getElementById("id you search for"); // return the element 
let myTagElement = document.getElementsByTagName("tag name like p for paragraph"); // return array of elements whose tag you search for
let myClassElement = document.getElementsByClassName("class name we search for"); // return array of elements whose class name you search for
let myQueryElement = document.querySelector(".special"); // select by id or tag or class any thing , but it return first element only
let myQueryAllElements = document.querySelectorAll(".special"); // return list of elements 
console.log(document.title);
console.log(document.body);
console.log(document.forms[i].nameofinput.value);// return list of form elements
console.log(document.links[i].href);
```

Dom Get / Set elements content and Attributes
```js
// innerHtml
// textContent
// Change Attributes directly
// change attributes with methods
// getAttribute
// setAttribute
document.images[0].src = "url";
console.log(document.images[0].getAttribute("class"));
console.log(document.getElementsByTagName("p")[0].attributes);

let myp = document.getElementsByTagName("p")[0];
if(myp.hasAttribute("data-src")){
	if(myp.getAttribute("data-src") === ""){
		myp.removeAttribute("data-src");
	}else
		myp.setAttribute("data-src" , "new value");
}

if(mpy.hasAttributes()){
	console.log("has attributes");
}

let myElement = document.createElement("div");
let myAttr = document.createAttribute("data-custom");
let myText = document.createTextNode("product one");

myElement.className = "product";
myElement.setAttributeNode(myAttr);

// append text to element
myElement.appendChild(myText);

// append element to boyd
document.body.appendChild(myElement);
```

Deal with children
- children
- childNodes
- firstChild
- lastChild
- firstElementChild
- lastElementChild

```js
let myElement = document.querySelector("div");
console.log(myElement.children); // get children elements
console.log(myElement.children[0]);
console.log(myElement.childNodes);// get children elements , text , comments
// firstChild ---> get first Node
// lastChild ---> get last node
// firstElementChild --> children[0]
// lastElementChild --> children[children.length - 1];
```

DOM Events
- we can use events on HTML
- we can use Events on JS
	- onclick : left click
	- oncontextmenu : right click
	- onmouseenter : when mouse click 
	- onmouseleave : when mouse leave
	- onload
	- onscroll : when I am scrolling the page
	- onresize
	- onfocus
	- onblur
	- onsubmit
```js
let myBtn = document.getElementById("btn");
myBtn.onclick = function(){
	console.log("clicked");
};
```

form validation
```js
document.forms[0].onsubmit = function(e){
	let uservalid = false;
	let agevalid = false;
	let userInput = document.querySelector("[name = 'username']");
	let ageInput = document.querySelector("[name = 'age']");
	if(userInput.value.length <= 10 && userInput.value.length > 0)
		uservalid = true;
	if(ageInput.value !== "") 
		agevalid = true;
	if(!agevalid || !uservalid){
		e.preventDefault();
	}
}
document.links[0].onclick = function(event){
	event.preventDefault(); 
}
```

event simulation
```js
let two = document.querySelector(".two");
window.onload = function(){
	two.focus();
}
let one = document.querySelector(".two");
one.onblur = function(){
	document.links[0].click();
}
```

Class List
- contains("class name") : true or false
- item("index of the class") : return class name
- add("add-one" , "add-two" , ...) : add classes
- remove("add-one" , "add-two" , ...)
- toggle ("class name") : add if not exist otherwise erase

Style elements from JS
```js
let element = document.getElementById("my-div");
element.style.color = "red";
element.style.backgroundColor = "blue";
element.style.cssText = "color : red; font-weight : bold; background-color : blue;";
element.style.removeProperty("color");
element.style.setPropoerty("color" , "red");
element.style.setPropoerty("color" , "red" , "important");
document.styleSheets[0].rules[0].style.removeProperty("line-height");
```

deal with elements
```js
let element = document.getElementById("my-div");
let createdp = document.createElement("p");
element.before("hello");
element.appendChild(createdp);
element.after(createdp);
element.append("hello"); // in the element at last
element.prepend("hello"); // in the element at front
element.remove(); // delete this element from the dom tree
```

Traversing elements in DOM
- nextSibling
- previousSibling
- nextElementSibling
- previousElementSibling
- parentElement

```js
let span = document.querySelector(".two");
console.log(span.nextElementSibling); // return the next element in same parent
span.onclick = function(){
	span.parentElement.remove();
}
```

DOM cloning
```js
let myP.querySelector("p").cloneNode(is it deep);// if true take all with children , otherwise take the element only with empty body
let myDiv = document.querySelector("div");
myDiv.appendChild(myP);
```

Add event lister
```js
let myP = document.querySelector("p");
myP.onclick = one;
myP.onclick = two;
function one(){
	console.log("Message from on click 1");
}
function two(){
	console.log("Message from on click 2");
}
myP.addEventListener("click" , one);
myP.addEventListener("click" , two); // equvilent to the above , it help when you want to add event to element not already on the page
document.addEventListener("click" , function(e){
	if(e.target.className === "clone"){
		console.log("Iam cloned");
	}
})
```
---------
BOM : Browser object model
- alert (message) -> need no response only ok available
- confirm (message) -> need response and return Boolean
- prompt (message , default message) -> collect data

```js
// confirm
let confirmedMessage = confirm("are you sure?");
if(confirmedMessage){
	console.log("confirm");
}else{
	console.log("not confirmed");
}
// alert
alert("hello , world");
// prompt
let promptMsg = prompt("write your answer." , "place holder");
```

set time out just do it once after the period pass while set Interval will do it every period
```js
let handler = setTimeout(function(){
	// the function you call after the period of time
	console.log("message");
} , 3000 , arguments you will pass for the function); // after 3000 ms call it
let btn = document.querySelector("button");
btn.onclick = function(){
	clearTimeout(handler); 
}
let handler2 = setInterval(function(){} , 3000); 
clearInterval(handler2);
```

location object
```js
console.log(location);
console.log(location.href);
location.href = "https://google.com";
location.href = "/#section2"; // send to the id directly
console.log(location.hostname); // name only
console.log(location.host); // name of the site , port number
console.log(location.protocol); // http or https
location.reload(); // reload the page
location.replace(new url); // change the url and remove the old from history
location.assign(new url); // same as replace but old url still in history
```

open , close methods
```js
window.close();  // close windows open by your js
window.open(url , widnowName , features)
```

history object
```js
console.log(history); // array of pages in history
history.go(x); // go x steps forward
```

scrolling methods
- stop() --> stop loading the resources
- print() --> print the page content
- focus() --> myNewWindow.focus() --> focus on it
- scroll (x , y || options) , scrollTo(x , y || options) --> make you at (x , y)
- scrollBy(x , y || options) ---> move you by offset (x , y)
- scrollX() --> how much you scroll horizontally
- scrollY() --> how much you scroll vertically

Local storage
Local storage in JavaScript allows you to store key-value pairs in a web browser with no expiration date. This means that the data persists even after the browser is closed and reopened, making it useful for storing user preferences, session data, or any other information that needs to be retained across sessions.
```js
// set
window.localStorage.setItem(key , value);
window.localStorage.key = value;
window.localStorage[key] = value;
// get
window.localStorage.getItem(key);
window.localStorage.key;
winodw.localStorage[key];
window.localStorage.key(index);
// remove
window.localStorage.removeItem(key);
window.localStorage.clear();
```

Session Storage
**Session storage** in JavaScript is a web storage object that allows you to store key/value pairs in the browser for the duration of a page session. This means that the data stored in session storage is only available for the current session and is deleted when the browser tab or window is closed.

-----------------------------------
Destructuring
```js
let myFriends = ["ahmed" , "mohamed" , "mohamed"];
let [a , b , c] = myFriends;
[a , b , c] = myFriends;
[a , b] = myFriends; // ahmed , mohamed
[a , , b] = myFriends; // ahmed , mohamed

let a = 2 , b  = 3;
// swapping
[a , b] = [b , a];

let user = {
var1 : "",
var2 : "",
...
}
let {var1 , var2 ,...} = user;
({var1 , var2 ,...} = user);
let {var1 : a , var2 : b , ...} = user;
// now a has the value of var , ...
```

---
Set
```js
let mySet = new Set(iterable); // array or anything
mySet.size;// size of the set
mySet.add(value); // adding an element
mySet.delete(value); // erase value
mySet.has(value); // boolean if exist
```

Set Vs WeakSet
the Weak Set is weak , meaning references to objects in a weak set are held weakly , if no other references to an object stored in the weak set exist , those objects can be garbage collected

set -> store any data values
weak set -> collection of objects only

Set -> have size property
weak set -> does not have size property

set -> have keys , values , entries
weak set -> does not have clear , keys , values and Entries

set -> can use forEach
weak set -> can't use forEach

```js
let s = WeakSet([{A : 1 , B : 2}]);
```
----
Map
syntax : new Map (Iterable with key / value)
map order by insertion order
```js
let map = new Map(iterable);
map.set(10 , 10); // insert
map.set("10" , 11);

map.get(10); // get the element
map.size;
let x = new Map([[19 , 11] , [23 , 2435]]);
x.delete(19); // delete the key
x.clear();
map.has(key); // check if key exist
```
---
Array Methods
```js
Array.from(Iterable , MapFunction , This)
Array.copyWithin(Target , start->optional , end->optional);
let x = [2 , 3];
x.some(boolean function(Element , Index , Array) , This);
x.every(boolean function(Element , Index , Array) , This);

//let x = object.keys(object you want it's keys to become array);
/* spread operator => ...Iterable */
console.log(..."khaled");
let arr = [..."khaled"];
// concat arrays
let x = [2 , 3 , 5];
let y = [2 , 34 , 5];
let all = [...x , ...y];
let obj = {
1 : 2
2 : 3
4 : 4
}
let obj = {
10 : 2
20 : 3
40 : 4
}
console.log({...obj , ...obj2}); // {1 : 2 , 2 : 3 , 4 : 4 , 10 : 2 , 20 : 3 , 40 : 4}
```
-----------
Regular Expression
- Syntax
	- /pattern/modifier(s);
	- new RegExp("pattern" , "modifier(s)")
- Modifier
	- i -> case insensitive
	- g-> global , get all results
	- m->multi lines
- Search Methods
	- match(pattern)
- Match
	- Matches A string against a regular expression pattern
	- returns an array with the matches
	- return null if no match is found
- Ranges
	- (X|Y) -> X or Y
	- [0-9] -> 0 to 9
	- [^0-9] -> any character not 0 to 9
	- [somestring] -> consider only those characters
	- [a-zA-Z] -> capital and small
- character classes 
	- . -> matches any character , except new line or other line terminators
	- \w -> matches word characters , [a-z , A-Z , 0-9 and underscore]
	- \W -> matches Non word characters
	- \d -> matches digits from 0 to 9
	- \D -> matches non-digit characters
	- \s -> matches white spaces characters
	- \S -> matches non-white space character
	- \b -> matches at the beginning or end of a word (skdfjsdf\b check end)
	- \B -> matches not at the beginning /end of a word
- Test method
	- pattern.test(input string)
- Quantifiers
	- n+ -> one or more
	- n* -> zero or more
	- n? -> zero or one
	- n{x} -> number of 
	- n{x , y} -> range
	- n{x , } -> at least x
	- $ -> End with something
	- ^ -> start with something
	- ?= -> followed by something
	- ?! -> not followed by something
- replace
	- string.replace("pattern" , "string");
- replaceAll
	- string.replaceAll("pattern" , "string");
- form validation
	- you can put rules for the values in fields

```js
let s = "my string El hello el";
let regex = /Elzero/;
console.log(s.match(regex));
regex = /elzero/ig; // case instensive , global
```
----
OOP
Constructor Function : function that creates objects
```js
function User(id , username , salary){
	this.id = id;
	this.username = username;
	this.salary = salary;
}
let userOne = new User(1 , "khaled" , 200);
console.log(userOne.id);
// -------- defining a class --------
class User{
	static count = 0; // only class can access , shared among objects
	constructor(id , username , salary){
		this.id = id;
		this.username = username;
		this.salary = salary;
		this.msg = function(){
			return `Hello ${this.u}`;
		}
	}
	writeMsg(){
		// function but not need key word function
		return `Hello ${this.u}`;
	}
	static sayHello(){
		return `Hello`;	
	}
}
let userTwo = new User(2 , "mohamed" , 200);
console.log(userTwo instanceof User); // check the class created from
console.log(userTwo.constructor === User);
// -------- inhertance --------
class Admin extends User{
	constructor(id , username , salary , permissions){
		super(id , username , salary);
		this.p = permissions;
	}
}
// -------- private properties --------
class User{
	// private property
	#private_variable;
	constructor(id , username , salary){
		this.i = id;
		this.u = username;
		this.#private_variable = eSalary;
	}
}
```
---------
Prototype 
In JavaScript, **`prototype`** is a special object that is used to share properties and methods across instances of a constructor function. It is part of JavaScript's **prototype-based inheritance** system.
Every JavaScript function automatically has a **`prototype`** property (except arrow functions). This property is an object that becomes the **prototype of all instances** created by that function when used with the `new` keyword.

```js
function Person(name) {
  this.name = name;
}

Person.prototype.sayHello = function () {
  console.log(`Hello, my name is ${this.name}`);
};

const alice = new Person("Alice");
const bob = new Person("Bob");

alice.sayHello(); // Hello, my name is Alice
bob.sayHello();   // Hello, my name is Bob
```
In this example:
- `sayHello` is defined on `Person.prototype`.
- Both `alice` and `bob` have access to `sayHello()` via the prototype chain.

When you access a property or method on an object:
1. JavaScript checks if it exists **on the object itself**.
2. If not, it checks the object’s **`[[Prototype]]`**, which is set to the constructor's `prototype`.
3. This continues up the chain (until `null`).
```js
console.log(alice.__proto__ === Person.prototype); // true
```

use cases
- Share methods and properties across all instances.
- Avoid duplicating methods in memory for each instance.
- Support inheritance (via `Object.create()` or classes).
---
Object Meta Data and Descriptor
```js
const myObject = {
	a: 1,
	b: 2,
};
Object.defineProperty(myObject , "c" , {
	writable : true ,
	enumerable : true,
	configurable : true, // if false , you can erase it or redefine
	value : 3,
});
for (let key in myObject){
	console.log(key , myObject[prop]);
}
delete myObject.c; // erase the property from the object and return T/F
// add more than one
object.defineProperties(myObject , {
	a : {
		// do the descriptor
		writeable : true,
		// the rest
	}, 
	b : {
		// same
	}
});
console.log(object.getOwnPropertyDescriptor(myObject , "d"));
console.log(object.getOwnPropertyDescriptors(myObject));
```
---
Date and Time
```js
let dateNow = new Date();
console.log(dateNow);
console.log(Date.now()); // number of ms from 1/1/1970
let birthday = new Date("oct 25, 82");
let dateDiff = dateNow - birthday;
/*
getTime -> number of milliseconds from 1/1/1970
getDate -> day of the month
getFullYear() -> 
getMonth() -> zero based like jan -> 0
getDay-> day of the week 0-> sunday
getHours()
getMinutes()
getSeconds()
*/
console.log(dateNow.getTime());
console.log(dateNow.getDate());
```
---
Generator Functions
- Generator functions Run its code when required
- Generator functions return special object (generator object)
- Generators are Iterable
```js
function* generateNumber(){
	yield 1;
	yield 2;
	yield 3;
	yield 4;
}
function* generateLetters(){
	yield "a";
	yield "b";
}
function* generateAll(){
	yield* generateNumber();
	yield* generateLetters();
	yield [2 , 3 , 45]; // it will return 1 array one time not 2 , 3 , 45
	yield* [2 , 3 , 45]; // return 2 , 3 , 45
	return "x"; // it same as generator.return();
}
let generator = generateNumber(); // generatorNumber().next() does affact this variable state
console.log(typeof generator);
console.log(generator.next()); // it has 2 attrbutes done , value
// done is false if not reached last yield , otherwise it is true
for (let value of generatorNumber()){
	console.log(value);
}
generator.return(); // stop the generator job
```
---
Modules: Import and Export
let file main
```js
export let a = 10;
export let arr = [1 , 2 , 3 , 4];
export function saySomething(){
	return "something";
}
// or I can do this
export{
	a , arr as z , saySomething
}; // the user when export arr should say z
export default function sayHello(){
	return "hello";
}
```

let app file
```js
import sayHello , {a as b , arr , saySomething} from "./main.js";
// default you can name it any thing , {named exports}
import * as all from "./main.js";
// import every thing from main.js file and to access you use all.
console.log(b);
```

HTML file
```html
<html>
	<head>
	</head>
	<body>
		<script src = "main.js" type = "module"></script>
		<script src = "app.js" type = "module"></script>
	</body>
</html>
```
---
JSON
it is JavaScript object Notation , format for sharing data between Server and Client , derived from  JavaScript , and it is an alternative to XML , JSON is a file extension
- text based format
- light weight
- does not use tags
- shorter
- can use arrays
- Not support comments

Syntax
- Data added inside curly braces
- data added with key: value
- key should be string wrapped in Double quotes
- Data separated by comma
- square brackets for arrays
- curly braces for objects
Available Data types
- String
- Number
- Object
- Array
- Boolean values
- null

```json
{
	"string" : "khalwsh" ,
	"number" : 100 , 
	"object" : {
		"EG" : "ggg",
		"ED" : "ggg",
	},
	"array"  : [2 , 3 , 4 , 5 , 6],
}
```
---
API : application programming interface
```js
// JSON.parse -> convert text data to JS object
// JSON.stringify -> convert JS object to JSON
const fromServer = '{"Username" : "khaled"}';
const jsObject = JSON.parse(fromServer);
const toServer = JSON.stringify(jsObject);
```

Asynchronous vs Synchronous Programming
in synchronous programming operations runs in sequence , each operation must wait for the previous one to complete , while the Asynchronous programming operation runs in parallel , this means that an operation can occur while another one is still being processed. 

Call Stack ,  stack trace
- javaScript Engine uses a call stack to manage execution contexts
- Mechanism to make the interpreter track your calls
- when function called it added to the stack
- when function executed it removed from the stack
- after function is finished executing the interpreter continue from the last point work using LIFO principle 
- work using LIFO principle 
- code execution is Synchronous
- call stack detect web API methods and leave it to the browser to handle it'
Web API
- Methods available from the Environment -> browser

Event Loop and Callback Queue
- java Script is a single Threaded Language 
- call stack track all calls
- every function is done its poped out
- when you call Asynchronous function it sent to browser API
- Asynchronous function like settimeout start its own thread
- Browser API act as a Second thread
- API finish waiting and send back the function for processing
- Browser API add the callback to callback queue
- event loop wait for call stack to be empty
- event loop get callback from callback queue and add it to call stack
- callback queue follow 
---
AJAX
- AJAX short for : Asynchronous JavaScript and XML
- Approach to use many technologies together (HTML , CSS , JS , DOM)
- It use XMLHttpRequest object to interact with the server
- You can fetch data or send data without page refresh
```js
let req = new XMLHttpRequest();
console.log(req);
```

Ready State -> status of the request
	0 -> request not Initialized
	1 -> server connection Established
	2 -> request Received 
	3 -> processing request
	4 -> Request is finished and response is read
Status
	200 -> response is successful
	404 -> Not found
```js
let myRequest = new XMLHttpRequest();
myRequest.open(method , url); // method "GET" , "POST" , "FETCH" , ...
myRequest.send(); // send the request
myRequest.onreadstatechange = function(){
	console.log(myRequest.readyState);
	console.log(myRequest.status);
	if(this.readyState === 4 && this.status === 200){
		console.log(this.responseText);
	}
};
```
---
**Callback Hell** (also known as the **Pyramid of Doom**) is a situation in asynchronous programming—especially common in JavaScript—where multiple nested callbacks make the code difficult to read, understand, and maintain.

A **callback** is a function passed as an argument to another function, which is then called (or "called back") later.

```js
doSomething(function (err, result1) {
  if (err) return handleError(err);
  doSomethingElse(result1, function (err, result2) {
    if (err) return handleError(err);
    doThirdThing(result2, function (err, result3) {
      if (err) return handleError(err);
      // and so on...
    });
  });
});
```

This leads to:
- Deeply nested code (like a pyramid ⛰️),
    
- Harder debugging and tracing,
    
- Poor error handling and logic branching,
    
- Low code readability.

 Solutions to Callback Hell:
1. **Named Functions**: Move callbacks into separate named functions.
    
2. **Promises**: Allow chaining `.then()` instead of nesting.
    
3. **Async/Await**: Makes asynchronous code look and behave like synchronous code.
    
4. **Libraries**: Like `async.js` help structure async flow.

Promise
A **Promise** in JavaScript is a way to handle asynchronous operations—like fetching data from a server—without falling into **callback hell**.

A **Promise** is an object that represents the **eventual completion (or failure)** of an asynchronous operation and its resulting value.

A Promise has three states:

1. **Pending** – the initial state, neither fulfilled nor rejected.
    
2. **Fulfilled** – the operation completed successfully.
    
3. **Rejected** – the operation failed.

```js
const promise = new Promise((resolve, reject) => {
  // async operation
  if (success) {
    resolve("It worked!");
  } else {
    reject("Something went wrong.");
  }
});
```

You use `.then()` to handle success, `.catch()` to handle errors, and `.finally()` to run code regardless of the result.

```js
promise
  .then(result => {
    console.log("Success:", result);
  })
  .catch(error => {
    console.log("Error:", error);
  })
  .finally(() => {
    console.log("Done!");
  });
```

Why Use Promises?
- Cleaner code compared to nested callbacks
    
- Better error handling
    
- Easy to chain multiple async operations
    
- Integrates well with `async/await`

```js
async function fetchData() {
  try {
    const result = await delay(1000);
    console.log(result);
  } catch (error) {
    console.error(error);
  }
}
```

---
Fetch API : return a representation of the entire HTTP response
```js
fetch(url).then((res) => {
	console.log(res);
	let data = res.json();
	console.log(data);
	return data;
}).then((data) => {
	data.length = 10;
	return data;
}).then((data) => {
	console.log(data[0]);
});
```
---
Async/await
In JavaScript, **`async`** is a keyword used to define asynchronous functions—functions that operate asynchronously via the **event loop**, returning a **Promise** implicitly, and allowing the use of the **`await`** keyword inside them.
```js
async function myFunction() {
  return "Hello";
}
```
This is equivalent to:
```js
function myFunction() {
  return Promise.resolve("Hello");
}
```
Because JavaScript is **single-threaded**, long-running tasks (like network requests, file reading, timers) would block the main thread. To avoid this, we use **asynchronous operations**—like promises—and `async/await` provides a cleaner way to work with them.

You can only use `await` inside an `async` function. It pauses the function execution until the Promise resolves (or rejects).
```js
async function getData() {
  const response = await fetch("https://api.example.com/data");
  const data = await response.json();
  console.log(data);
}
```
- `await fetch(...)` waits until the fetch completes.
- Execution inside `getData` is paused at each `await`, but the rest of the program keeps running.
```js
async function getUser() {
  try {
    const res = await fetch("/api/user");
    if (!res.ok) throw new Error("Failed to fetch");
    const user = await res.json();
    return user;
  } catch (err) {
    console.error("Error:", err);
  }
}
```

with Promises : 
```js
fetch("/api/user")
  .then(res => res.json())
  .then(user => console.log(user))
  .catch(err => console.error(err));
```

With async/await
```js
async function getUser() {
  try {
    const res = await fetch("/api/user");
    const user = await res.json();
    console.log(user);
  } catch (err) {
    console.error(err);
  }
}
```

