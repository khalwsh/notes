What Are Servlets?
- java class that is processed on the server
- java class generates HTML that is returned to browser
- can read HTML form data , use cookies , sessions ,...
- At high-level , similar to JSPs
![[Pasted image 20250307163600.png]]

Example for web App using Servlets
![[Pasted image 20250307163710.png]]

to start working with servlets , you must add java class that extends servlet and to make sure your jsp project recognize java  , the project structure must be like this
![[Pasted image 20250307174857.png]]

then mark the java directory as the Sources Root by right click on it then mark as then Sources root , then - **Go to** `File → Project Structure → Modules`
- **Check the "Sources" tab**:
    - **Make sure `src/main/java` is marked as "Sources" (blue folder)**.
- **Go to the "Dependencies" tab**:
    - Make sure an SDK (e.g., Java 11) is selected.

---------
JSP and Servlets comparision
![[Pasted image 20250308063854.png]]

servlets was released 1997 while JSPs was released 1999.
Best practice
- integrate them both together
	- Servlet does the business logic
	- JSP handles the presentation view
- Model-View-Controller (MVC) Design Pattern

Reading HTML Form data using servlets 
![[Pasted image 20250308162720.png]]

![[Pasted image 20250308162743.png]]

![[Pasted image 20250308162822.png]]
![[Pasted image 20250308162842.png]]

Difference between POST , GET HTTP methods
Sending Data with GET method the data get appended to the URL as name , value pair while in POST method the data passed in the body of the HTTP request message
![[Pasted image 20250308164222.png]]

Servlet configuration parameters
- Your web app can make use of configuration parameters
- Located in standard file: WEB-INF/web.xml
ex:
![[Pasted image 20250308164556.png]]

![[Pasted image 20250308164818.png]]
note that the context always return string and if the variable not exist it return null