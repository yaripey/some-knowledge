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


---

## HTTP Connections
HTTP uses the TCP transport protocol to make the connection between the client and server. A TCP stream is broken into IP packets, and it ensures that those packets always arrive in the correct order without fail. HTTP is an application layer protocol over TCP, which is over IP.

HTTPS is a secure version of HTTP, inserting an additional layer between HTTP and TCP called TLS or SSL (Transport Layer Security or Secure Sockets Layer). HTTPS communicates over port 443 by default.

Establishing a connection between two endpoints is a multi-step process and invloves the following:
1. Resolve IP address from host name via DNS
2. Establish a connection with server
3. Send a request
4. Wait for a response
5. Close connection

Before HTTP/1.1, all connection were closed after a single transaction. To reduce connection-establishment delays, HTTP/1.1 introduced __persisten connections__, long-lived connections that stay open until the client closes them. They are default in HTTP/1.1 and making a single transaction connection requires the client to set the `Connection: close` request header.

### Server-side Connection Handling
The server mostly listens for incoming connections and processes them when it receives a request. The operations involve:
1. Establishing a socket to start listening on port 80 (or other)
2. Receiving the request and parsing the message
3. Processing the response
4. Setting response headers
5. Sending the response to the client
6. Close the connection if a `Connection: close` request header was found

---

## Identification and Authentication

It is almost mandatory to know who connects to a server for tracking an app's or site's usage and the general interaction patterns of users.

There are a few different ways a server can collect this information, and most websites use a hybrid of these approaches:
- __Request headers__: `From`, `Referer`, `User-Agent`
- __Client-IP__ - the IP address of the client
- __Fat Urls__ - storing state of the current user by modifying the URL and redirecting to a different URL on each click; each click essentially accumulates state.
- __Cookies__ - the most popular and non-intusive approach

Cookies allow the server to attach information for outgoing response via the `Set-Cookie` response header. For example: `Set-Cookie: sessino-id=12345ABC; username=testtest`. 

A server can also restrict the cookies to a specific `domain` and `path`, and it can make them persistent with an `expires` value. Cookies are automatically sent by the browser for each request made to a server ,and the browser ensures that only the `domain`- and `path`-specific cookies are sent in the request. The request header `Cookie: name-value [; name2=value2]` is used to send these cookies to the server.

### Authentication

HTTP does support a rudimentary form of authentication called __Basic Authentication__, as well as the more secure __Digest Authentication__.

In Basic Authentication, the server initially denies the client's request with a `WWW-Authenticate` response header and a `401 Unauthorized` status code. On seeing this header, the browser displays a login dialog, prompting for a username and password. This information is sent in a base-64 encoded format in the `Authentication` request header. The server can now validate the request and allow access if the credentials are valid. Some server might also send an `Authentication-Info` header containing additional authentication details.

Digest Authentications basicaly does the same thing, but in a more secure way. Still, websites typically use Basic Authenticatoin because of its simplicity.

### Certificates
One of the requirement of starting HTTPS server is getting a certificate. These are issued by a Certificate Authority (CA) and vouch for your identity on the web. The CAs are the guardians of the PKI. The most common form of certificates is the _X.509 v3 standard_, which contains information, such as:

- the certificate issuer
- the algorithm used for the certificate
- the subject name or organization for whom this cert is created
- the public key information for the subject
- the Certification Authority Signature, using the specified signing algorithm.

When a client makes a request over HTTPS, it first tries to locate a certificate on the server and verify it against its known list of CAs. 

---

## HTTP Caching

Caches are used at several places in the network infrastructure, from the browser to the origin server. Depending on where it is located, a cache can be categorized as:
- __Private__: within a browser, caches usernames, passwords, URLs, browsing history and web content. 
- __Public__: deployed as caching proxies between the server and client. 

### Cache Processing
The process of maintaining a cache is quite similar:

- __Recieve__ request message
- __Parse__ the URL and headers
- __Lookup__ a local copy; otherwise, fetch and store locally
- Do a __freshness check__ to determine the age of the content in the cache; make a request to refresh the content only if necessary
- Create the __response__ from the cached body and updated headers
- __Send__ the response back to client
- Optionally, __log__ the transaction