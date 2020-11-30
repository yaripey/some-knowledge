# HTTP notes

## General

HTTP stands for Hypertext Transfer Protocol.

HTTP is a __stateless__ protocol. The communication usually takes place over TCP/IP. The default port for TCP/IP is __80__.

Communication between a host and a client occurs, via a __request/response pair__. The client initiates and HTTP request message, which is serviced through a HTTP response message in return.

---

## Request Verbs

The action that should be performed on the host is specified via HTTP verbs. These verbs are:
- __GET__: fetch an existing resource. The URL contains all the necessary information the server needs to locate and return the resource.
- __POST__: create a new resource. POST requests usually carry a payload that specifies the data for the new resource.
- __PUT__: update and existing resource. The payload may contain the updated data for the resource.
- __DELETE__: delete an existing resource.

---

## Status Codes

With URLS and verbs, the client can initiate requests to the server. In return, the server respons with status codes and message payloads. The status code is important and tells the client how to interpret the server response.

### 1xx: Informational Messages

This class of codes is purely provisional.

### 2xx: Successfujl

This tells the client that the request was successfully processed. The most common code is __200 OK__. For a ```GET``` request, the server sends the resource in the message body. There are other less frequently used codes:

- __202__ Accepted: the request was accepted but may not include the resource in the response. This is useful for async processing on the server side. The server may choose to send information for monitoring.
- __204__ No Content: there is no message body in the response.
- __205__ Reset Content: indicates to the client to reset its document view.
- __206__ Partial content: indicates that the response only contains partial content. Additional headers indicate the exact range and content expiration information.

### 3xx: Redirection

This requires the client to take additional action. The most common use-case is to jump to a different URL in order to fetch the resource.

- __301__ Moved Permanently: the resource is now located at a new URL.
- __303__ See Other: the resource is temporarily located at a new URL. The `Location` response header contains the temporary URL.
- __304__ Not Modified: the server has determined that the resource has not changed and the client should use its cached copy. This relies on the fact that the client is sending `ETag` (Entity Tag) information that is a hash of the content. The server compares this with its own computed `ETag` to check for modifications.

### 4xx: Client Error

These codes are used when the server thinks that the client is at fault, either by requesting an invalid resource or making a bad request. The mos popular code is __404 Not Found__.

- __400__ Bad Request: the request was malformed.
- __401__ Unauthorized: request requires authrentication. The client can repeat the request with the `Authorization` header. If the client already included the `Authorization` header, then the credentials were wrong.
- __403__ Forbidden: server has denied access to the resource.
- __405__ Method Not Allowed: invalid HTTP verb used in the request line, or the server does not supoprt that verb.
- __409__ Conflict: the server could not complete the request because the client is trying to modify a resource that is newer thatn the client's timestamp. conflicts arise mostly for PUT requests during collaborative edits on a resource.

### 5xx: Server Error

This class of codes are used to indicate a server failure while processing the request. The most commonly used error code is __500 Internal Server Error__. The others in this class are:

- __501 Not Implemented__: the server does not yet support the requested functionality.
- __503 Service UNavailable__: this could happen if an internal system on the server has failed or the server is overloaded. Typically, the server won't even respond and the request will timeout.

## Request and Response Message Formats

The HTTP specification states that a request or response message has the following generic structure:
```
message = <start-line>
          *(<message-header>)
          CRLF
          [<message-body>]

<start-line> = Request-Line | Status-Line 
<message-header> = Field-Name ':' Field-Value
```

It's mandatory to place a new line between the message headers and body. the message can contain one or more headers, of which are broadly classified into:
- general headers
- request specific headers
- response specific headers
- entity headers

### General Heaeders

These are shared by both request and response messages:

- `Via` header is used in a TRACE message and updated by all intermittent proxies and gateways
- `Pragma` is considered a custom ehader and may be used to include implementation-specific headers
- `Date` header is used to timestamp the request/response message
- `Upgrade` is used to switch protocols and allow a smooth transition to a newer protocol
- `Transfter-Encoding` is generally used to break the response into smaller parts with the `Transfer-Encoding: chunked` value.

### Entitiy Headers

Request and Response messages may include entity headers to provide meta-information about the content. These include:
- Content-Encoding
- Content-Language
- Content-Length
- Content-Location
- Content-MD5
- Content-Range
- Content-Type
- Expires
- Last-Modified

### Request Headers

The request headers act as modifiers of the request message.

The `Accept` prefixed headers indicate the acceptable media-types, languages and character sets on the client. `From`, `Host`, `Referer` and `User-Agent` identify details about the client that initiated the request.

### Request Format
Example of the request message:

```
GET /articles/http-basics HTTP/1.1
Host: www.articles.com
Connection: keep-alive
Cache-Control: no-cache
Pragma: no-cache
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
```

### Response Headers
- `Age` is the time in seconds since the message was generated on the server.
- `ETag` is the MD5 hash of the entity and used to check for modifications.
- `Location` is used when sending a redirection and contains the new URL.
- `Server` identifies the server generating the message.