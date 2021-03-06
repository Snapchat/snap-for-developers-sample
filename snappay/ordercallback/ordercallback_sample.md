In order to invoke the **canvas ordercallback** you will first need to get a server to server auth token with the target id of ```canvas-ordercallback```.
Take a look at **server-to-server-auth/sample.md** for more details.

Once you have a server to server auth token you are ready to invoke the **canvas ordercallback**.

Create a **POST** Request and set the following headers:
  - Authorization -> ```Bearer <server to server auth token> // note the space between Bearer and the auth token```
  - Content-Type -> ```application/json```

The body of the post request has the following format:
```
{
  "query": "mutation($o: UpdateOrderRequest!) {UpdateOrder(order: $o)}", 
  "variables":{
    "o" : <serialized UpdateOrderRequest>
  }
}
```

Execute the request against the callback url: 
```
https://us-central1-gcp.api.snapchat.com/ordercallback/graphql
```

A successful response (200 status code) will have the following format:
```
{
  "data": {
    "UpdateOrder": true
  }
}
```

Here is an example **POST** request body:
```
 {
    "query":"mutation($o: UpdateOrderRequest!) {UpdateOrder(order: $o)}",
    "variables":{
       "o":{
          "appID":"f38a4850-52f8-11ec-bf63-0242ac130002",
          "nonce":"payment-nonce",
          "externalUserID":"external_user_id",
          "orderStatus":"SUCCESS",
          "details":{
             "externalOrderID":"1802c98c-52f9-11ec-bf63-0242ac130002",
             "shippingAddress":{
                "address1":"2772 Donald Douglas Loop North",
                "address2":"",
                "city":"Santa Monica",
                "state":"CA",
                "country":"US",
                "zip":"90405",
                "firstName":"John",
                "lastName":"Smith"
             },
             "contact":{
                "email":"email",
                "phone":"phone"
             },
             "shippingOption":{
                "handle":"standard",
                "price":{
                   "amount":0,
                   "currency":"USD"
                },
                "title":"order_processor_title"
             },
             "subtotalPrice":{
                "amount":4.99,
                "currency":"USD"
             },
             "totalTax":{
                "amount":1.58,
                "currency":"USD"
             },
             "totalPrice":{
                "amount":6.57,
                "currency":"USD"
             },
             "externalOrderName":"order_processor_9700bda2-52f9-11ec-bf63-0242ac130002",
             "billingPurchaseState":"COMPLETE",
             "billingItems":[
                {
                   "itemInfo":{
                      "productID":"a454e7da-52f9-11ec-bf63-0242ac130002",
                      "productTitle":"Product Name",
                      "variantID":"0",
                      "quantity":1,
                      "unitPrice":{
                         "amount":4.99,
                         "currency":"USD"
                      },
                      "totalCost":{
                         "amount":4.99,
                         "currency":"USD"
                      },
                      "requiresShipping":false,
                      "taxable":true
                   },
                   "productImageUrl":"https://my.images.com/appicon.png"
                }
             ]
          },
          "storeID":"b882f95e-52f9-11ec-bf63-0242ac130002"
       }
    }
 }
```