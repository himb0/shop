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

The code for implementation of authentication is taken from, [raml.org/raml-1.0](https://raml.org/developers/whats-new-raml-10). This API  allows only get, post, put, and delete requests; any other requests like patch, trace are deemed invalid and returned 

> 405 - A request method is not supported for the requested resource.

**Header Details**
> Authorization

> Tenant-Id

> Content-type

> x-ratelimit-limit: the rate limit ceiling for that given endpoint. Each client is provided a 5 min window

> x-ratelimit-remaining: the number of requests left for the 5 minute window

> x-ratelimit-reset: the remaining window before the rate limit resets, in UTC epoch seconds

> User-Agent: 
Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:47.0)Gecko/20100101 Firefox/47.0                       Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko)                   
Chrome/51.0.2704.103 Safari/537.36; Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36; Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko)
Chrome/51.0.2704.106 Safari/537.36 OPR/38.0.2220.41
Mozilla/5.0 (iPhone; CPU iPhone OS 10_3_1 like Mac OS X) AppleWebKit/603.1.30
(KHTML, like Gecko) Version/10.0 Mobile/14E304 Safari/602.1
Mozilla/5.0 (compatible; MSIE 9.0; Windows Phone OS 7.5; Trident/5.0 EMobile/9.0)

> Accept-Language: en-US;en-UK

- Use Case 1
This use case provides consumer's personal information to the requesting external entities. 

	**x-rate-limit-limit:** the rate limit ceiling for that given endpoint. Each client is provided a 5 min window
    
	**x-rate-limit-remaining:** the number of requests left for the 5 minute window
    
	**x-rate-limit-reset:** the remaining window before the rate limit resets, in UTC epoch seconds
    
    The API fulfills the client's request and provides customer details like first name, last name, and address. Besides servicing client with regards to customer information, it further provides order, and product details which will be discussed in Use Case 3. 

### **Design Considerations**




