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

This API requires OAUTH2 authentication with requesting clients to service their requests. 

> baseUri: https://api.dropbox.com/{version}
version: v1
mediaType: application/json
securitySchemes:
    oauth_2_0:
      description: |
          Dropbox supports OAuth 2.0 for authenticating all API requests.
      type: OAuth 2.0
      describedBy:
          headers:
              Authorization:
                  description: |
                     Used to send a valid OAuth 2 access token. Do not use
                     with the "access_token" query string parameter.
                  type: string
          queryParameters:
              access_token:
                  description: |
                     Used to send a valid OAuth 2 access token. Do not use
                     together with the "Authorization" header
                  type: string
          responses:
              401:
                  description: |
                      Bad or expired token.
              403:
                  description: |
                      Bad OAuth request (wrong consumer key, bad nonce, expired
                      timestamp...).
      settings:
        authorizationUri: https://www.dropbox.com/1/oauth2/authorize
        accessTokenUri: https://api.dropbox.com/1/oauth2/token
        authorizationGrants: [ authorization_code, password ]


- Use Case 1
This use case provides consumer's personal information to the requesting external entities. 

	**x-rate-limit-limit:** the rate limit ceiling for that given endpoint. Each client is provided a 5 min window
    
	**x-rate-limit-remaining:** the number of requests left for the 5 minute window
    
	**x-rate-limit-reset:** the remaining window before the rate limit resets, in UTC epoch seconds
    
    The API fulfills the client's request and provides customer details like first name, last name, and address. Besides servicing client with regards to customer information, it further provides order, and product details which will be discussed in Use Case 3. 

### **Design Considerations**




