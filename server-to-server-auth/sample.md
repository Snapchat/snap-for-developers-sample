In order to get a server to server auth token, the following steps must be completed.

Create a **POST** request using **Basic Auth**.

Basic Auth
- username ```appID```
- password ```application secret received from partner engineering```

POST Request URL
```
https://us-central1-gcp.api.snapchat.com/s2stoken/graphql
```

Set the Request **Content-Type** to **application/json**

POST Request Body
```
{
  "query":"query VendS2SAuthTokenQuery($targetId: String!){VendS2SAuthToken(targetServiceID: $targetId)}",
  "variables":{
    "targetId": <target id>
  }
}
```

Set the ```<target id>``` to the snap service you wish to call.  For example, if you intend to call the **canvas-ordercallback** service then the target id would be set to **canvas-ordercallback**.
This results in a request body of 
```
{
  "query":"query VendS2SAuthTokenQuery($targetId: String!){VendS2SAuthToken(targetServiceID: $targetId)}",
  "variables":{
    "targetId": "canvas-ordercallback"
  }
}
```

For a successful response the status code will be **200** and the body will look like the following:

```
{
  "data": {
    "VendS2SAuthToken": "<s2s token>"
  }
}
```

You now have successfully requested a server to server auth token that can be used to call snap services.