#### What are Web Applications?

A web site where the HTML pages are generated dynamically based on the user's actions

![[Pasted image 20250221142045.png]]

#### What are JSPs and Servlets?
JSPs & Servlets are java code that runs on the web server , reads user's actions ... normally from HTML form then performs the work and return an HTML page that is generated dynamically.

#### What types of Web Apps can you create?
- Any industry
- E-commerce
- student / Employee Tracking
- Restaurant / Hotel Reservations
- Social Media
- ..... (NO LIMITS)

JSPs and Servlets are key components of the Java Enterprise Edition (java EE)
Popular MVC frameworks (Spring , JSF , Struts) are actually built on top of JSP and Servlets

#### Required software
- JDK
- Java Application Server (Tom cat require JRE)
- IDE


##### What is a JSP file?
An HTML page with some java code sprinkled in , include dynamic content from java code.
![[Pasted image 20250223023549.png]]

##### Where is the JSP processed?
JSP is processed on the server , Results of java code included in HTML returned to browser
![[Pasted image 20250223023931.png]]

##### Where to place JSP file?
The JSP file goes in your Web Content folder , and it Must have .jsp extension
![[Pasted image 20250223024108.png]]

##### code example
```jsp
<html>
<body>

<h3> Hello World of java! </h3>

The time on the server is <%= new java.util.Date() %>

</body>
</html>
```

so we write normal HTML code and when we want to write java code we do in this tag
```jsp
<%= // java code %>
```

##### JSP Scripting Elements
![[Pasted image 20250223025147.png]]

JSP Expression: compute an expression , Result is included in HTML returned to browser
```jsp
<%= some Java expression %>
```

JSP Scriptlets: Insert 1 to many lines of Java code , used to include content in the page.
```jsp
<% 
// lines of code ex: out.println("print to the web page")
%>
```

for the best practice Minimize the amount of Scriptlet code in a JSP , Avoid dumping thousands of lines of code in JSP , Refactor this into separate java class and make use of MVC.

##### JSP Declarations
it allow to Declare a method in the JSP page , call the method in the same JSP page
```jsp
<%!
// declare a method
%>
```
 same best practice of Scriplets

##### calling java class from jsp
 Create the Java Class
1. In IntelliJ, locate the `src/main/java` folder (if you’re using Maven or a similar structure).
2. Right-click on the package (e.g. `com.example`) → **New** → **Java Class**.
3. Give your class a name (e.g., `MyClass`) and click **OK**.
4. Add Your class in it

 Reference the Class in Your JSP

Let’s assume you have a JSP file named `index.jsp` in `src/main/webapp`. In that JSP, you can import and use your `MyClass`:
```jsp
<%@ page import="com.example.MyClass" %>
```

or 

```jsp
<%@ page import="com.example.MyClass , another package , ... " %>
```

##### JSP Built-In Server Objects
![[Pasted image 20250223035939.png]]

![[Pasted image 20250223040139.png]]

we can use those objects for example
```jsp
 <%= request.getHeader("user-Agent") %> // knowing the os
```

##### Including Files
![[Pasted image 20250223041404.png]]

You put where you want to load this page
```jsp
<jsp:include page = "my-header.html" /> // example of adding file
```

##### What Does EL (Expression Language) Do in JSP?

Expression Language (EL) in JSP makes it easier to access data stored in **JavaBeans, request/session/application attributes, and collections** without writing complex Java code inside JSP files.
```jsp
<%@ page isELIgnored="false" %>
```
#### Review HTML Forms
HTML forms are used to get input from user
![[Pasted image 20250224021530.png]]

##### building HTML Form
we specify the form tag and the action (tell where to send the form data)
```JSP
<form action = "jsp-input form">  
    First name : <input type = "text" name = "first name"/>  
    Last name : <input type = "text" name = "last name"/>  
    <input type = "submit" , value = "submit"/>  
</form>
```

How to read the data in the JSP file? we use the request object + the filed name you defined in the HTML input tag
```jsp
The student name <%= request.getParameter("firstName") %>
```

or we can do this

```jsp
The student name : ${param.firstName} ${param.lastName}
```

if the browser ignore the second method use this
```jsp
<%@ page isELIgnored="false" %>
```

##### building Drop-Down list 
![[Pasted image 20250224023609.png]]

```html
<select name = "Country">
<option> Egypt </option>
<option> France </option>
<option> Germany </option>
....
</select>
```


##### Radio Button 
![[Pasted image 20250224031327.png]]

```HTML
C++ <input type = "radio" name = "favoriteLanguage" value = "cpp">  
JAVA <input type = "radio" name = "favoriteLanguage" value = "java">  
  C# <input type = "radio" name = "favoriteLanguage" value = "C#">  
RUBY <input type = "radio" name = "favoriteLanguage" value = "Ruby">
```

##### Check box 
![[Pasted image 20250224034154.png]]

```HTML
C++ <input type =  "checkbox" name = "favoriteLanguage" value = "cpp">  
JAVA <input type = "checkbox" name = "favoriteLanguage" value = "java">  
  C# <input type = "checkbox" name = "favoriteLanguage" value = "C#">  
RUBY <input type = "checkbox" name = "favoriteLanguage" value = "Ruby">
```

We are getting alot of values back so we will use getParameterValues that return array of strings
```JSP
<%  
    String[] x = request.getParameterValues("fav");  
    for (String s : x)  
        out.println(<li>s</li>); // print as list element
%>
```

-----------------
##### JSP Session object
JSP session is created once for user's browser session. Unique for this user , commonly used when you need to keep track of user's actions.

![[Pasted image 20250226011605.png]]

Adding data to session object
```jsp
session.setAttribute(String name , Object value)
```

for example
```jsp
List<String> items = new ArrayList<>();
session.setAttribute("myToDoList" , items);
```

Retrieve data from session object
```jsp
Object session.getAttribute(String name)
```

for example
```jsp
List<String> myStuff = (List<String>) session.getAttribute("myToDoList");
```

![[Pasted image 20250226012050.png]]

Example for making To dolist and using Session (note the sesssion is unique for the browser user)
```jsp
<%@ page import="java.util.List" %>  
<%@ page import="java.util.ArrayList" %>  
<html>  
    <title>        khalwsh  
    </title>  
    <body>        <form action = "index.jsp">  
            Add new item :  
  
            <label>  
                <input type = "text" name = "item"/>  
            </label>  
            <label>                <input type = "submit" value = "submit">  
            </label>        </form>  
        <%  
            List<String> currentList = (List<String>) session.getAttribute("ToDoList");  
            if (currentList == null) {  
                currentList = new ArrayList<String>();  
                session.setAttribute("ToDoList" , currentList);  
            }            if(request.getParameter("item") != null){  
                currentList.add(request.getParameter("item"));  
                session.setAttribute("ToDoList" , currentList);  
            }        %>  
  
        <hr>  
        <b> To Do List items: </b>  
        <br>  
        <ol>            <%  
                for (String item : currentList) {  
                    out.println("<li>" + item + "</li>");  
                }            %>  
  
        </ol>  
  
    </body></html>
```

--------------
What is the Purpose of Cookies?
Cookies personalize a web site for a user and keep track of user preferences 

What is a Cookie?
Text data exchange between web browser and server
![[Pasted image 20250226022110.png]]

what is Cookies content? 
it is key-value pair 
![[Pasted image 20250226022153.png]]

How are cookies passed?
Browser will only send cookies that match the server's domain name

Cookie API - Package
cookie class defined in package: javax.servlet.http , by default it is imported
```jsp
<%Cookie (String name , String value) // constructs a cookie with the specified name and value
%>
```

Example for sending Cookies to the Browser
```jsp
<% String favLang = request.getParameter("favoriteLanguage");
// create cookie
Cookie theCookie = new Cookie("myApp.favoriteLanguage" , favLang);
// set life span ... total number of seconds
theCookie.setMaxAge(60 * 60 * 24 * 365);
// send cookie to browser
response.addCookie(theCookie); >
```

Example reading cookies from the browser
```jsp
<%
String favLang = "java";
Cookie[] theCookies = request.getCookies();
if(theCookies != null){
	for(Cookie tempCookie: theCookies){
		if("myApp.favoriteLanguage".equals(tempCookie.getName())){
			favLang = tempCookie.getValue();break;
		}
	}
}
%>
```

-------------------------------------------------
#### JSP Tags
- JSP Custom Tags
	- Move heavy business logic into supporting class
	- Insert JSP custom tag to use supporting class
	- Minimize the amount of Scriptlet code in JSP
	- Avoids dumping thousands of lines of code in a JSP
	- JPS page is simple ... main focus of JSP is only the presentation
	- Tag is reusable
- JSP Standard Tag Library (JSTL)
	- Oracle created a specification for standard tags (ex: core , message formatting , xml , functions , SQL , ....)

how to reference the core tags of JSTL library
```jsp
<%@ taglib uri = "http://java.sun.com/jsp/jstl/core" prefix = "c" %>
```

example of using tag to set variable
```jsp
<c:set var = "stuff" value = "<%= new java.util.Date() %>"/>
Time on the server is ${stuff}
```

##### JSTL core
JSTL (Java Server Pages Standard Tag Library) core tags are a set of custom tags that simplify common tasks in JSP pages, reducing the need for embedded Java code. These tags make your pages cleaner, more maintainable, and easier to read by handling logic operations, data formatting, iteration, and more through simple XML-like syntax.

Variable Management:
**`<c:set>`:**  
This tag sets a variable (in a specified scope) that can later be used throughout the page.
```jsp
<c:set var="username" value="John Doe" />
```
**`<c:remove>`:**  
Removes a variable from a specified scope.


output and Expression Handling
**`<c:out>`:**  
Safely outputs a value to the page (helping to avoid XSS vulnerabilities by escaping HTML by default).
```jsp
<c:out value="${username}" />
```

Conditional logic
**`<c:if>`:**  
Evaluates a condition and includes its body only if the condition is true.  
```jsp
<c:if test="${contditions}">
    Welcome, <c:out value="${username}" />!
</c:if>
```

**`<c:choose>`, `<c:when>`, and `<c:otherwise>`:**  
These tags work together to implement switch-case like logic. , add when tags as you like
```jsp
<c:choose>
    <c:when test="${userRole == 'admin'}">
        Admin Dashboard
    </c:when>
    <c:otherwise>
        User Dashboard
    </c:otherwise>
</c:choose>
```

Iteration
**`<c:forEach>`:**  
Iterates over collections, arrays, or a range of numbers, rendering the body content for each item.
```jsp
<c:forEach var="item" items="${itemList}">
    <p>${item}</p>
</c:forEach>
```
**`<c:forTokens>`:**  
Splits a string into tokens based on a delimiter and iterates over these tokens.

URL Handling
**`<c:url>`:**  
Helps to construct URLs properly by including session identifiers when needed, which is especially useful for maintaining sessions in environments without cookies.
```jsp
<c:url var="loginUrl" value="/login.jsp" />
<a href="${loginUrl}">Login</a>
```

Example of using JSTL core tags
```jsp
<%@ taglib = "http://java.sun.com/jsp/jstl/core" prefix = "c" %>
<%
	String []cities = {"benha" , "alex" , "cairo"};
	pageContext.segAttribute("myCities" , cities);
%>
<html>
	<body>
		<c:forEach var = "tempCity" items = "${myCities}">
		${tempCity} <br/>
		</c:forEach>
	</body>
</html>
```

##### JSTL function tag
prefix "fn"
- Collection Length
	- length
- String manipulation
	- toUpperCase , toLowerCase
	- substring , substringAfter , substringBefore
	- trim , replace , indexof , startsWith , endsWith
	- contains , containsIgnoreCase , split , join , escapeXml

to include Function tags must use this reference
```jsp
<%@ taglib uri = "http://java.sun.com/jsp/jstl/functions" prefix = "fn" %>
```

```jsp
<c:set var = "data" value = "khalwsh" />
Length of the string is ${fn:length(data)}
upper case version : ${fn:toUpperCase(data)}
start with : ${fn:startsWith(data , prefix)}
```

fn:split(): splits a string into an array of substrings based on delimiter
```pesudocode
String[] split(String data , string delimiter)
```

```jsp
<c:set var = "data" value = "Singapore,Tokyo,Mumbai,London" />
<c:set var = "cities array" value = "${fn:split(data , ',')}" />
```

fn:join() : concatenates a string array into single String based on a delimiter
```pesudocode
String join(String[]data , String delimiter)
```

```jsp
<c:set var = "fun" value = "${fn:join(citiesArray , '*')}" />
result of joining : ${fun}
```

##### JSTL nationalization 
it is basically the process when design application to make it adapt any language without change the source code (change language in website)

what is I18N?
the term internationalization is frequently abbreviated as I18N  which is equivalent start + length + end 

instead of hard coding display text / messages in your application , we make use of labels / the placeholders
![[Pasted image 20250307011900.png]]

you need to create translated version of each label , based on user's language selection , system will include the appropriate text

Locale = language + region
en_US = English(US)
en_GB = English(UK)
....
others

also formatting like date formatting
![[Pasted image 20250307020835.png]]

![[Pasted image 20250307022215.png]]

Create Resource File
- Translated versions of your labels
	- we will need Google translate or similar translator service
- must name the file using locale

what is locale?
Locale = Language code + country code

file naming :
```
<your project name>_LANGUAGECODE_COUNTRYCODE.properties
```
for example , file: mylabels_es_ES.properties
```jsp
label.greeting = Hola
label.firstname = Nombre de pila
label.lastname = Apellido
label.welcom = Bienvenidos a la clase de formacion
```

Create JSP page with labels
```jsp
<fmt:message key = "label.greeting" /> <br/><br/>
<fmt:message key = "label.firstname" /> <i>John</i> <br/>
<fmt:message key = "label.lastname" /> <i>Doe</i> <br/>
<fmt:message key = "label.welcome" /> <br/>
```

update jsp page to change locale based on user selection
- we pass the locale as a parameter
```jsp
<a href = "test.jsp?theLocale=en_US"> English(US) </a>
<a href = "test.jsp?theLocale=es_Es"> Spanish(ES) </a>
```

Update JSP page to change locale based on user selection
```jsp
<c:set var = "theLocale"
	value = "${not empty param.theLocale ? param.theLocale : pageContext.request.locale}" scope = "session" />
<fmt:setLocale value = "${the Locale}" />
<fmt:setBundle basename = "test"/>
```

Example for the locale usage
![[Pasted image 20250307033321.png]]

my label file is the default one
![[Pasted image 20250307033352.png]]

