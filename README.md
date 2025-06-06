![ascii-text-art](https://github.com/Dambylik/42_Webserv/blob/main/photos/webservm.png)

## Project Overview
The goal of the project is to build a C++ 98 compatible HTTP web server from scratch.
The web server can handle HTTP GET, POST and DELETE Requests, and can serve static files from a specified root directory or dynamic content using CGI. It is also able to handle multiple client connections concurrently with the help of epoll().
<br>

---
# Usage
```bash
make
./webserv config/webserv.conf
```
## Screenshots of our project

| | |
|:-------------------------:|:-------------------------:|
| <img width="1230" alt="Screen Shot 2024-05-09 at 12 22 47 PM" src="https://github.com/Dambylik/42_Webserv/blob/main/photos/Britney%20Groupies.png"> | <img width="1245" alt="Screen Shot 2024-05-09 at 12 24 31 PM" src="https://github.com/Dambylik/42_Webserv/blob/main/photos/Britney%20Sparkle%20Chat.png">
| <img width="1346" alt="Screen Shot 2024-05-09 at 12 47 45 PM" src="https://github.com/Dambylik/42_Webserv/blob/main/photos/Your%20Britney%20Playlist.png"> | <img width="1288" alt="Screen Shot 2024-05-09 at 12 40 14 PM" src="https://github.com/Dambylik/42_Webserv/blob/main/photos/Terminal.png"> |

## Introduction

HTTP (Hypertext Transfer Protocol) is a protocol for sending and receiving information over the internet. It is the foundation of the World Wide Web and is used by web browsers and web servers to communicate with each other.

An HTTP web server is a software application that listens for and responds to HTTP requests from clients (such as web browsers). The main purpose of a web server is to host web content and make it available to users over the internet.

HTTP consists of requests and responses. When a client (such as a web browser) wants to retrieve a webpage from a server, it sends an HTTP request to the server. The server then processes the request and sends back an HTTP response.

HTTP Message can be either a request or response.

## HTTP Request

An HTTP request consists of a request line, headers, and an optional message body. Here is an example of an HTTP request:
```
GET /index.html HTTP/1.1
Host: localhost:8080
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)
```
The request line consists of three parts: the method, the path, and the HTTP version. The method specifies the action that the client wants to perform, such as GET (to retrieve a resource) or POST (to submit data to the server). The path or URI specifies the location of the resource on the server. The HTTP version indicates the version of the HTTP protocol being used.

Headers contain additional information about the request, such as the hostname of the server, and the type of browser being used.

In the example above there was no message body because GET method usually doesn't include any body.

## HTTP Response

An HTTP response also consists of a status line, headers, and an optional message body. Here is an example of an HTTP response:

```
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1234

<Message Body>
```

The status line consists of three parts: the HTTP version, the status code, and the reason phrase. The status code indicates the result of the request, such as 200 OK (successful) or 404 Not Found (resource not found). The reason phrase is a short description of the status code. Following is a very brief summary of what a status code denotes:

`1xx` indicates an informational message only

`2xx` indicates success of some kind

`3xx` redirects the client to another URL

`4xx` indicates an error on the client's part

`5xx` indicates an error on the server's part

Headers contain additional information about the response, such as the type and size of the content being returned. The message body contains the actual content of the response, such as the HTML code for a webpage.


## HTTP Methods

|Method|Description|Possible Body|
|:----|----|:----:|
|**`GET`** | Retrieve a specific resource or a collection of resources, should not affect the data/resource| No|
|**`POST`** | Perform resource-specific processing on the request content| Yes|
|**`DELETE`** | Removes target resource given by a URI| Yes|

__GET__

HTTP GET method is used to read (or retrieve) a representation of a resource. In case of success (or non-error), GET returns a representation of the resource in response body and HTTP response status code of 200 (OK). In an error case, it most often returns a 404 (NOT FOUND) or 400 (BAD REQUEST).

__POST__

HTTP POST method is most often utilized to create new resources. On successful creation, HTTP response code 201 (Created) is returned.

__DELETE__

HTTP DELETE is stright forward. It deletes a resource specified in URI. On successful deletion, it returns HTTP response status code 204 (No Content).
<br>

Read more about HTTP methods [RFC9110#9.1](https://www.rfc-editor.org/rfc/rfc9110.html#name-methods)
<br>

# Parts of a web server

A basic HTTP web server consists of several components that work together to receive and process HTTP requests from clients and send back responses. Below are the main parts of our webserver.

## Server Core
The networking part of a web server that handles TCP connections and performs tasks such as listening for incoming requests and sending back responses. It is responsible for the low-level networking tasks of the web server, such as creating and managing sockets, handling input and output streams, and managing the flow of data between the server and clients.

## Request Parser

The parsing part of a web server refers to the process that is responsible for interpreting and extracting information from HTTP requests.
In this web server, the parsing of requests is performed by the HttpRequest class. An HttpRequest object receives an incoming request, parses it, and extracts the relevant information such as the method, path, headers, and message body(if present). If any syntax error was found in the request during parsing, error flags are set and parsing stops.
Request can be fed to the object through the method feed() either fully or partially, this is possible because the parser scans the request byte at a time and update the parsing state whenever needed. The same way of parsing is used by Nginx and Nodejs request parsers.

## Response Builder

The response builder is responsible for constructing and formatting the HTTP responses that are sent back to clients in response to their requests.
In this web server, the Response class is responsible for building and storing the HTTP response, including the status line, headers, and message body.
The response builder may also perform tasks such as setting the appropriate status code and reason phrase based on the result of the request, adding headers to the response to provide additional information about the content or the server, and formatting the message body according to the content type and encoding of the response.
For example, if the server receives a request for a webpage from a client, the server will parse the request and pass it to a Response object which will fetch the contents of the webpage and construct the HTTP response with the HTML content in the message body and the appropriate headers, such as the Content-Type and Content-Length headers.

## Configuration File

Configuration file is a text file that contains various settings and directives that dictate how the web server should operate. These settings can include things like the port number that the web server should listen on, the location of the web server's root directory, and many other settings.

## CGI

CGI is a standard for running external programs from a web server. When a user requests a web page that should be handled by a CGI program, the web server executes the program and returns the output to the user's web browser.

CGI programs are simply scripts that can be written in any programming language, such as Perl, Python, or bash, and are typically used to process data submitted by a user through a web browser, or to generate dynamic content on a web page.

<p align="center">
  <img width="60%" height="50%" src="https://i1.ae/img/webserv/CGI.jpg">
</p>
