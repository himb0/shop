# **The RESTful API Modeling Language (RAML) Spec**

A RAML Spec exposes consumer's  information. 

## **Use Case Coverage** 

This API is covering below use cases:-
1. A consumer may periodically (every 5 minutes) consume the API to enable it (the consumer) to maintain a copy of the provider API's customers (the API represents the system of record)

2. A mobile application used by customer service representatives that uses the API to retrieve and update the customers details
 
3. Simple extension of the API to support future resources such as orders and products

### **How does API work?**

RAML spec provides customer records - personal information, their order history as well as lists product catalog available in shop. This API has a parent class: [Shop API](https://github.com/himb0/shop/blob/master/shop.raml) which stores shop objects and uses its child class objects to display relevant information. 



### **Design Considerations**




