# HTTP

HTTP (Hypertext Transfer Protocol) is the foundation of web communication between client and server. It follows a request-response model where the web browser (chrome, firefox, etc.) - acting as the client - sends an HTTP request to a server that is hosting
the website, and the server processes the request and sends back an HTTP response.

HTTP is a stateless protocol, meaning each request is treated independently and the server does not remember anything about previous requests. 
This reduces server load and keeps the protocol simple and scalable.

Common HTTP methods include:
- `GET`: retrieve resources from the server (HTML, JSON, images, etc.).
- `POST`: send data to the server, usually to create a new resource
- `PUT`: update or replace existing resource on the server.
- `PATCH`: partially update existing resource.
- `DELETE`: delete a resource from the server.

### Http request structure
An HTTP request is essentially what the client sends to request the server for something. Here's a basic structure:
- **Request method**: `GET`, `POST`, `PUT`, `DELETE`
- **Headers**: key-value pairs for additional information like `Content-Type`, etc.
- **Body**: mainly used with `POST`, `PUT`, or `PATCH` to send data to the server (JSON or form data format).
- Example request:
```
  GET /api/users/1 HTTP/1.1
  Host: example.com
  Accept: application/json
```

### HTTP response structure
The server returns a response to the client after processing a request. Basic structure:
- **Status**: includes status code such as `200 OK` or `400 Bad Request`.
- **Headers**: metadata such as `Content-Type`, `Content-Length`, and so on.
- **Body**: content returned by the server (HTML, JSON, etc.).
- Example response:
```
  HTTP/1.1 200 OK
  Content-Type: application/json
  Content-Length: 38
  
  { "id": 1, "name": "User" }

```

### status code categories
- 1xx — Informational
- 2xx — Success (e.g., 200 OK, 201 Created)
- 3xx — Redirection
- 4xx — Client errors (e.g., 400 Bad Request, 404 Not Found)
- 5xx — Server errors (e.g., 500 Internal Server Error)

### HTTPS (HTTP + TLS)
Modern websites use a more secure version of HTTP referred to as HTTPS (Hypertext Transfer Protocol Secure) to protect login details and any sensitive data.
It's used to encrypt communication between the client and server using TLS (Transport Layer Security). This prevents attackers from reading
or tampering with data during transmission.

### Idempotent HTTP methods
`GET`, `UPDATE` and `DELETE` are idempotent, meaning they produce the same result regardless of how many times the request has been repeated.
Unlike `POST` requests for example, which create additional changes every time they're triggered.

### Practical Test Using cURL
Let's use `curl` command to send an HTTP request directly from the terminal. If we run this command from the terminal:
```shell
  curl -i https://jsonplaceholder.typicode.com/posts/1
```
we'll get a response containing status code, headers and response. Response body looks like this:
```json
  {
    "userId": 1,
    "id": 1,
    "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
    "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto"
  }
```

Thanks for reading!
