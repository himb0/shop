# **The RESTful API Modeling Language (RAML) Spec**

A RAML Spec exposes consumer's  information. 

## **Use Case Coverage** 

This API is covering below use cases:-
1. A consumer may periodically (every 5 minutes) consume the API to enable it (the consumer) to maintain a copy of the provider API's customers (the API represents the system of record)

2. A mobile application used by customer service representatives that uses the API to retrieve and update the customers details
 
3. Simple extension of the API to support future resources such as orders and products

### **How does API work?**

RAML spec provides customer records - personal information, their order history as well as lists product catalog available in the shop. This API has a parent class: [Shop API](https://github.com/himb0/shop/blob/master/shop.raml) which stores shop attributes and uses its child class attributes to display further information about customer's ordering history and the range of products in the catalog.

[RAML Class Diagram](https://www.lucidchart.com/documents/view/8de59b44-6450-46a2-ac11-64c8a59cb8e7)

The idea is to provide customer sensitive information privately and in a secure channel to avoid any eavesdropping or masquerading of data. This API requires OAUTH2 authentication with requesting clients to service their requests. API returns 

> 401 - Bad or expired token 
> 403 - Bad oauth request (wrong consumer key, bad nonce, expired timestamp..}.

The code for implementation of authentication is taken from, [raml.org/raml-1.0](https://raml.org/developers/whats-new-raml-10). This API  allows only: 

> get

> post

> put

> delete

> patch/trace/connection: deemed invalid requests and return, 405 - A request method is not supported for the requested resource.

**Header Details**
> [hasHeader] - The header name, defined as trait for re-using in GET, POST, PUT, DELETE methods. 
> Authorization: OAUTH2 authentication base 64 encoded string

> Tenant-Id: Tenant id for multi-tenant environments

> Content-type: json

> x-ratelimit-limit: the rate limit ceiling for that given endpoint. Each client is provided a 5 min window

> x-ratelimit-remaining: the number of requests left for the 5 minute window

> x-ratelimit-reset: the remaining window before the rate limit resets, in UTC epoch seconds

> User-Agent: 
Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:47.0)Gecko/20100101 Firefox/47.0
Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko)                   
Chrome/51.0.2704.103 Safari/537.36; Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36; Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko)
Chrome/51.0.2704.106 Safari/537.36 OPR/38.0.2220.41
Mozilla/5.0 (iPhone; CPU iPhone OS 10_3_1 like Mac OS X) AppleWebKit/603.1.30 (KHTML, like Gecko) Version/10.0 Mobile/14E304 Safari/602.1
Mozilla/5.0 (compatible; MSIE 9.0; Windows Phone OS 7.5; Trident/5.0 EMobile/9.0)

> Accept-Language: en-US;en-UK

## **Use Case 1**

### **List customers**
This use case provides consumer's personal information, products, and order history to the requesting external entities. The API fulfills the client's request through 
> GET: provides customer personal information - first name, last name, and address
This call is OAUTH2 secure, displays header information through trait: hasHeader. 

The GET method incorporates error handling through network layer,

> 400: The server cannot or will not process the request due to an apparent client error (e.g., malformed request syntax, size too large, invalid request message framing, or deceptive request routing).

> 408: The server timed out waiting for the request. According to HTTP specifications: "The client did not produce a request within the time that the server was prepared to wait. The client MAY repeat the       request without modifications at any later time"

> 423: The resource that is being accessed is locked

> 429: The user has sent too many requests in a given amount of time. Request limit per 5 minutes.
            
> 500: A generic error message, given when an unexpected condition was encountered and no more specific message is suitable.

> 503: The server is currently unavailable (because it is overloaded or down for maintenance). Generally, this is a temporary state.
              
> 504: The server was acting as a gateway or proxy and did not receive a timely response from the upstream server.
            
> 200: Request successful for listing the customer records as collection.

Besides servicing client with regards to customer information, it further provides order, and product details which will be discussed in Use Case 3. Rate limiting is incorporated at the network layer to handle congestion, and service clients in a distributed environment giving clients equal priority in round robin fashion. 

### **Create a new customer**
This use case creates a new customer(s) in a collection.
> POST: creates a customer record with first name, last name, and address
This call is OAUTH2 secure, displays header information through trait: hasHeader. 

The POST method incorporates error handling through network layer,

> 400: The server cannot or will not process the request due to an apparent client error (e.g., malformed request syntax, size too large, invalid request message framing, or deceptive request routing).

> 408: The server timed out waiting for the request. According to HTTP specifications: "The client did not produce a request within the time that the server was prepared to wait. The client MAY repeat the       request without modifications at any later time"

> 423: The resource that is being accessed is locked

> 429: The user has sent too many requests in a given amount of time. Request limit per 5 minutes.
            
> 500: A generic error message, given when an unexpected condition was encountered and no more specific message is suitable.

> 503: The server is currently unavailable (because it is overloaded or down for maintenance). Generally, this is a temporary state.
              
> 504: The server was acting as a gateway or proxy and did not receive a timely response from the upstream server.
            
> 201: Request successful for creating a new list of customer(s) in a collection.

Rate limiting is incorporated at the network layer to handle congestion, and service clients in a distributed environment giving clients equal priority in round robin fashion. 

### **Update a customer**
This use case fetches the customer id from the client and updates a customer information partially/completely in the collection. 

> PUT: updates a customer record with first name, last name, and address
This call is OAUTH2 secure, displays header information through trait: hasHeader. 

The PUT method incorporates error handling through network layer,

> 400: The server cannot or will not process the request due to an apparent client error (e.g., malformed request syntax, size too large, invalid request message framing, or deceptive request routing).

> 408: The server timed out waiting for the request. According to HTTP specifications: "The client did not produce a request within the time that the server was prepared to wait. The client MAY repeat the       request without modifications at any later time".

> 422: The request was well-formed but was unable to be followed due to semantic errors. 

> 423: The resource that is being accessed is locked.

> 429: The user has sent too many requests in a given amount of time. Request limit per 5 minutes.
            
> 500: A generic error message, given when an unexpected condition was encountered and no more specific message is suitable.

> 503: The server is currently unavailable (because it is overloaded or down for maintenance). Generally, this is a temporary state.
              
> 504: The server was acting as a gateway or proxy and did not receive a timely response from the upstream server.
            
> 200: Request successful for updating customer(s) in the collection.

Rate limiting is incorporated at the network layer to handle congestion, and service clients in a distributed environment giving clients equal priority in round robin fashion. 

### **Delete a customer**

## **Use Case 2**

## **Use Case 3**

### **Design Considerations**




