best practice is to use Servlet does the business logic and JSP handles the presentation view , we can combine between them using MVC
![[Pasted image 20250310134820.png]]

Benefits of MVC
- Minimizes HTML code in Servlet
	- no more: out.println() in Servlets code
- Minimize java business logic in JSPs
	- no more large scriptlets in JSP code

Servlet can call JSP using a request dispatcher
```java
// step1: get request dispatcher
RequestDispatcher dispatcher = request.getRequestDispatcher("path of jsp");
// step2: foward to the JSP
dispatcher.forward(request , response);
```

Sending Data to JSP 
```java
// add data
String [] arr = {"khaled"};
request.setAttribute("student_list" , arr);
// get request dispatcher , forward to jsp
RequestDispatcher dispatcher = request.getRequestDispatcher("path of jsp");
dispatcher.forward(request , response);
```

