#### 🔹HTTP protocol

- HTTP = Hypertext Transfer Protocol , it is an application layer protocol that allows web based applications to communicate and exchange data
- the HTTP is the messenger of the web
- It is a TCP / IP based protocol
- It is used to deliver contents , for example images , videos , audios , document , etc. 

#### 🔹 Three important things about the HTTP

1- HTTP is connectionless: after making the request , the client disconnect from the server , then when the response is ready the server re-establish the connection again and deliver the response

2- The HTTP can deliver any sort of data as long as the two computers are able to read it

3- The HTTP is a stateless : the client and server know about each other just during the current request , if it closes , and the 2 computers want to connect again , they need to provide information to each other anew , and the connection is handled as the very first one

#### 🔹 Why the HTTP?

- The HTTP was designed mainly to fetch html documents and sends it to the client 
- It was designed in  a exquisite way
- It as being continually evolved and features were being added to it
- It became the most convenient way to quickly and reliably move data on the web

#### 🔹 How the web works? and how Http makes that possible?
The web works by enabling communication between **clients** (such as web browsers) and **servers** through the **Hypertext Transfer Protocol (HTTP)**. This process allows users to access web pages, download files, and interact with online applications.

##### 🔹 How the Web Works 

1. **You Enter a URL:**
    
    - When you type a URL (e.g., `https://www.google.com`) in your browser, it needs to find the corresponding server.
2. **DNS Resolution:**
    
    - Your browser contacts a **Domain Name System (DNS)** server to convert the domain name (`www.google.com`) into an **IP address** (e.g., `142.250.182.14`).
3. **Establishing a Connection:**
    
    - Your browser uses the **IP address** to contact the **web server** hosting the website.
    - A **TCP (Transmission Control Protocol)** connection is established, often using **TLS (Transport Layer Security)** if HTTPS is used.
4. **Sending an HTTP Request:**
    
    - The browser sends an **HTTP request** to the server, asking for a webpage (e.g., `GET /index.html`).
5. **Processing the Request on the Server:**
    
    - The server receives the request and retrieves the necessary **HTML, CSS, JavaScript, images, and other resources**.
6. **Server Sends an HTTP Response:**
    
    - The server responds with an **HTTP response** containing:
        - **Status Code** (e.g., `200 OK` if successful, `404 Not Found` if the page doesn't exist).
        - **Content-Type** (e.g., `text/html` for an HTML page).
        - **Body** (the actual web page content).
7. **Rendering the Web Page:**
    
    - The browser parses the HTML, applies CSS for styling, executes JavaScript, and renders the page for you to see and interact with.
8. **Additional Requests for Resources:**
    
    - If the webpage contains images, scripts, or stylesheets, the browser makes **more HTTP requests** to load them.

---

##### 🔹 How HTTP Makes This Possible

**HTTP (Hypertext Transfer Protocol)** is the backbone of the web, allowing communication between browsers and servers. It works as a **request-response protocol**:

- **Client (Browser) sends an HTTP request** (e.g., `GET /index.html`).
- **Server processes the request** and sends a response.
- **Response contains a status code and the requested content**.

##### 🔹 HTTP Methods

HTTP uses **methods** to define the action:

- `GET` → Retrieve data (e.g., web pages, images).
- `POST` → Send data to the server (e.g., submitting a form).
- `PUT` → Update an existing resource.
- `DELETE` → Remove a resource.

##### 🔹 HTTP Status Codes

Servers respond with **status codes** indicating the result:

- `200 OK` → Successful request.
- `301 Moved Permanently` → Redirected to another URL.
- `404 Not Found` → Resource doesn't exist.
- `500 Internal Server Error` → Server encountered a problem.

##### 🔹 HTTPS (Secure HTTP)

- Uses **TLS (Transport Layer Security)** for encryption.
- Ensures secure communication by preventing **man-in-the-middle attacks**.
---------
