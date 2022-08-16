# Skytravel Customer Service API documentation
[toc]
## Common API 
This service provides several models to query, save, update and delete.
All models that provided are in the following list, you can easily add this model name in url after the port number with the query information following in order to request specific resource. For instance, if you are requesting address which id=1, use url http://127.0.0.1:8080/address/id=1
- customer
- address
- account
- document
- marriage
- documentType
- Notice: address, account, document, marriage are customer related, could query by customer Id.

### Find model by customer id
- Request Method: GET
- Request URL: /model-name/find/cid/{cid}
- Request URL sample: http://localhost:8080/address/find/cid/1
- Return list will be in **entity** key pair of response body and HTTP status: ok.
- Notice: Only address, account, document, marriage are supported query by customer id.

### Find model by id
- Request Method: GET
- Request URL: /model-name/find/id/{id}
- Request URL sample: http://localhost:8080/customer/find/id/1
- Return object will be in **entity** key pair of response body and HTTP status: ok.
### Delete model by id
- Request Method: DELETE
- Request URL: /model-name/delete/id/{id}
- Request URL sample: http://localhost:8080/customer/delete/id/1
- Return bool will be in **success** key pair of response body and HTTP status: ok.
### Update model by id
- Request Method: PUT
- Request URL: /model-name/update/id/{id}
- Request URL sample: http://localhost:8080/customer/update/id/1
- Request body should be send as JSON, JSON format would be in specific model API sections.
- Return Object value will be in **entity** key pair, if update success, return saved entity and HTTP status: ok.
### Create new model
- Request Method: POST
- Request URL: /model-name/new
- Request URL sample: http://localhost:8080/customer/new
- Request body should be send as JSON, JSON format would be in specific model API sections.
- Return object value will be in **entity** key pair, if create success, return saved entity and HTTP status: CREATED.

## Customer Information API
### POST/PUT request body: customer json format sample
```JSON
{
    "firstName":"James",
    "lastName":"Ande",
    "gender":"M",
    "dateOfBirth":"1971-08-06",
    "phone":"3473639136",
    "address":{"street":"691 61st st",
        "city":"brooklyn",
        "state":"ny",
        "zipCode":"11220"
        },
    "marriage":{
        "status":"SINGLE"
    },
    "document":[{
        "documentNumber":"123456789",
        "type":"1"
    }],
    "account":[{
        "title":"BOA",
        "account":"336685597",
        "password":"123456",
        "description":"Primary tax account"
    }]
}
```
#### What information is required to create a customer
- All customer primary information is required. Including following: first name, last name, gender, date of birth(must in form YYYY-mm-DD), phone.
- Other information is not required. 
- Other attribute notice: address and marriage must be write as an object with **{}**, if customer is new, please don't attach customer information inside of object. Account and document must be in form of an array, you can attach no matter how much you want when new customer initialize.
#### What information is required to update a customer
- The update operation only require customer's id and updated information in create format. This is a simple example that update none information for customer whose id = 1, {"id":"1"}. **It is not reccommanded to add marriage, document, account, and address attributes by update customer, they have their own update api**.
#### What information is showed in response of query a customer
 Return value: Return value is in the following json, all query data is in the **entity** key pair. If query is specific for a customer with unique identify (id), inside of data is an object, or if query with ambiguous attribute(name and birth, or phone number), the return value would be a list.

Response sample:
```JSON
 { 
    "entity":[
            {
                "id": "1",
                "firstName": "James",
                "middleName": null,
                "lastName": "Anderson",
                "phone": "6316338355",
                "gender": "",
                "dateOfBirth": "1970-01-07",
                "address": [
                    {
                        "id": "2",
                        "street": "599 91st st",
                        "city": "brooklyn",
                        "state": "ny",
                        "zipCode": "11220",
                        "primary": false
                    },
                    {
                        "id": "1",
                        "street": "419 21st st",
                        "city": "brooklyn",
                        "state": "ny",
                        "zipCode": "11220",
                        "primary": true
                    }
                ],
                "marriage": [
                    {
                        "id": "1",
                        "status": "SINGLE",
                        "startDate": null,
                        "createDate": "2022-08-07 10:16:33",
                        "lastModified": "2022-08-07 10:16:33"
                    }
                ],
                "account": [
                    {
                        "id": "4",
                        "title": "BOA",
                        "account": "336685597",
                        "password": "123456",
                        "description": "Primary tax account",
                        "updateTime": "2022-08-17 01:58:05"
                    }
                ],
                "document": [
                    {
                        "id": "2",
                        "type": "c8",
                        "documentNumber": "1456865",
                        "issuedDate": "1997-06-06",
                        "expireDate": "1998-06-06",
                        "createDate": "2022-08-08 03:42:26",
                        "updateTime": "2022-08-08 03:42:26"
                    },
                    {
                        "id": "1",
                        "type": "green card",
                        "documentNumber": "1456829",
                        "issuedDate": null,
                        "expireDate": null,
                        "createDate": "2022-08-07 10:16:33",
                        "updateTime": "2022-08-08 07:22:15"
                    }
                ],
                "createDate": "2022-08-07 10:16:33",
                "updateTime": "2022-08-08 07:17:30"
            }
        ]
    }
```


### Find customer
This is a base url to query the customer information.
- Request URL: /customer/find

#### Find customer by phone
- Request Method: GET
- Request URL: /customer/find/phone/{phone}
- Request URL sample: http://localhost:8080/customer/find/phone/3477075588

#### Find customer by customer first name
- Request Method: GET
- Request URL: /customer/find/firstname/{firstname}
- Request URL sample: http://localhost:8080/customer/find/firstname/James

#### Find customer by customer first name, last name
- Request Method: GET
- Request URL: /customer/find/fullname/{firstname}/{lastname}
- Request URL sample: http://localhost:8080/customer/find/fullname/James/Anderson
 
#### Find customer by customer first name, last name, and birthday
- Request Method: GET
- Request URL: /customer/find/namebirth/{firstname}/{lastname}?birth=YYYY-mm-DD
- Request URL sample: http://localhost:8080/customer/find/namebirth/James/Anderson?birth=1970-08-07
- Notice:  Request param must in format **YYYY-mm-DD**.
  

## Address Information API
### POST/PUT request body: address json format sample
```json
{
    "customer":"1",
    "street": "519 91st st",
    "city": "brooklyn",
    "state": "ny",
    "zipCode": "11220",
    "primary":"false"
}

```
### What information is required to create an address
- The basic street, city, zipcode, state, and customerId is required.
- The primary is default false, it is not required.
### What information is required to update an address
Only address id is required and should be written inside of json.
```json
{
    "id":"1",
    "street": "519 91st st",
    "primary":"true"
}
```
### What information is showed in response of query an address
The following is response of http://localhost:8080/address/find/id/1, 
```json
{
    "entity": {
        "id": "1",
        "street": "519 91st st",
        "city": "brooklyn",
        "state": "ny",
        "zipCode": "11220",
        "primary": true
    }
}
```

## Marriage Information API
### POST/PUT request body: marriage json format sample
```json
{
    "customer":"1",
    "status": "MARRIED",
    "startDate": "2022-08-01"
}

```
### What information is required to create a marriage
- The customer id, status is required.
- Status is enum only has **SINGLE, MARRIED, DEVORCED, WIDOWED** values.
### What information is required to update a marriage
Only marriage id is required and should be written inside of json.
```json
{
    "id":"7",
    "status": "SINGLE",
    "startDate": "2022-08-07"
}
```
### What information is showed in response of query a marriage
The following is response of http://localhost:8080/marriage/find/id/1, 
```json
{
    "entity": {
        "id": "1",
        "status": "SINGLE",
        "startDate": null,
        "createDate": "2022-08-07 10:16:33",
        "lastModified": "2022-08-07 10:16:33"
    }
}
```

## Document Type Information API
### POST/PUT request body: DocumentType json format sample
```json
{
    "title": "c08",
    "description": "IRS document"
}

```
### What information is required to create a document type
- Only title is required. Please check is there any same title object in database by calling http://localhost:8080/documentType/allDocumentType by GET method.
### What information is required to update a documentType
Only documentType id is required and should be written inside of json.
But it is not reccommanded to modify because it may cause data inaccuracy.
```json
{
    "id":"7",
    "title":"c09"
}
```
### What information is showed in response of query a documentType
The following is response of http://localhost:8080/marriage/find/id/1, if you call http://localhost:8080/documentType/allDocumentType, it would be a list of objects.
```json
{
    "entity": {
        "id": "1",
        "status": "SINGLE",
        "startDate": null,
        "createDate": "2022-08-07 10:16:33",
        "lastModified": "2022-08-07 10:16:33"
    }
}
```
### Find All Document Type 
- Request Method: GET
- Request URL:  documentType/allDocumentType
- Request URL sample:  http://localhost:8080/documentType/allDocumentType

## Document Information API
### POST/PUT request body: Document json format sample
```json
{
    "customer":"1",
    "type": "1",
    "documentNumber": "1456825",
    "issuedDate": null,
    "expireDate": null
}

```
### What information is required to create a document 
- Document number, customer and type(documentType.id) is required. Please check the 
### What information is required to update a document
Only document id is required and should be written inside of json.
```json
{
    "id":"7",
    "documentNumber":"123456"
}
```
### What information is showed in response of query a document
```json
{
    "entity": {
        "id": "1",
        "type": "c8",
        "documentNumber": "1456825",
        "issuedDate": null,
        "expireDate": null,
        "createDate": "2022-08-07 10:16:33",
        "updateTime": "2022-08-08 07:22:15"
    }
}
```

## Account Information API
### POST/PUT request body: Account json format sample
```json
{
    "customer":"1",
    "title": "boa",
    "account": "1456825",
    "password": "123",
    "description":"Primary bank account"
}

```
### What information is required to create a document 
- All attributes are required.
- If there is no password or description, pass empty string.
### What information is required to update a document
Only account id is required and should be written inside of json.
```json
{
    "id":"7",
    "password":"123456"
}
```
### What information is showed in response of query a document
```json
{
    "entity": {
        "id": "1",
        "title": "BOA",
        "account": "336685597",
        "password": "123456",
        "description": "Primary tax account",
        "updateTime": "2022-08-17 01:55:51"
    }
}
```