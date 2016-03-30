# Metro Challenge

<p align="center">
    <img alt="Metro Logo" src="http://logodatabases.com/wp-content/uploads/2012/05/metro-ag-logo.jpg" width="250px" />
</p>

## Challenge Description

Build the future of shopping! Or at least a small inspirational start which can be built on.

We (METRO Cash & Carry) will provide you access to the anonymized purchase history of 660 of our customers spanning over the last 6 months. Around 200 of these customers get goods delivered and the other visit our stores. The total number of transactions (1 article per line) spans over 2.2 million lines.

We're very eager to see what kind of creative ideas you can come up with in order to improve the experience of our customers, or METRO Cash & Carry, using this chunk of data. Some ideas we have are showing interesting infographics (either to us or to our customers) based on their shopping habits, recommendations, relevant marketing, the possibilities are endless.

## Authentification
In order to secure the API we use a JWT based authentification system.

You have to call the http://metro.food-hacks.de/rpc/login Route to get the generated JWT Token.

Afterwards you have to add this token to every request to the API. Details on how to get the token can be found in the following section.

## API Route description

For the Metro challenge you are able to query offline basket case data (sales orders) for Metro cash and carry customers. Therefore, the data model consists out of customer, sales orders and salesorder items. 

Details on the data model can be found in the route description.

The API is build with postgrest around a postgres sql database and is read-only. For details on how to filter results sets please refere to the [http://postgrest.com/api/reading/](postgrest documentation).

### Login
This route enables the basic authentification mechanism.
```
POST http://metro.food-hacks.de/rpc/login
Content-Type "application/json"
BODY 
{ "email": "metro@foodhacks.de", "pass": "WILL BE REVEALD AT FOODHACKS"}
```
The request will return an answer like the following

```
{
  "token": "JSON WEB TOKEN"
}
```
You need to store this token locally and use this for the future requests.

### Customers
Returns the list of customers present in the database. A customer is uniquely identifiey by the *customer_id*

```
GET /customers HTTP/1.1
Host: metro.food-hacks.de
Authorization: Bearer JSON WEB TOKEN

```
An example answer could look like this
```
[
  {
    "customer_id": "Client_0146",
    "home_store_id": 403
  },
  {
    "customer_id": "Client_0164",
    "home_store_id": 644
  }
]
```

### Sales Orders

Returns a list of sales orders with some summarization statics of this sales order based on the salesorder items for this particular order. A sales order is uniquely identified by the *invoice_id*

```
GET /salesorders HTTP/1.1
Host: metro.food-hacks.de
Authorization: Bearer JSON WEB TOKEN
```

An example answer could look like this
```
[
  {
    "invoice_id": "Invoice_020076",
    "customer_id": "Client_0164",
    "home_store_id": 644,
    "order_date": "2015-09-09",
    "turnover_inc_vat": 16.74,
    "amount_of_article": 1,
    "basket_size": 18
  },
  {
    "invoice_id": "Invoice_070787",
    "customer_id": "Client_0146",
    "home_store_id": 403,
    "order_date": "2016-01-01",
    "turnover_inc_vat": 3.7,
    "amount_of_article": 2,
    "basket_size": 9
  }
]
```

### Sales order items
This route returns a list of sales order items. Each sales order item represent a group of same articles of a sales order. A salesorder item is represented by its id.

```
GET /salesorder_items HTTP/1.1
Host: metro.food-hacks.de
Authorization: Bearer JSON WEB TOKEN
```

A example reponse could look like this:

````
[
  {
    "id": 1,
    "customer_id": "Client_0164",
    "invoice_id": "Invoice_020076",
    "home_store_id": 644,
    "sales_channel_id": 1,
    "sales_channel_description": "C&C",
    "order_date": "2015-09-09",
    "article_id": 423388,
    "article_name": "10er Eier weiss/braun M Boden",
    "amount": 18,
    "turnover_inc_vat": 16.74
  },
  {
    "id": 2,
    "customer_id": "Client_0146",
    "invoice_id": "Invoice_070787",
    "home_store_id": 403,
    "sales_channel_id": 1,
    "sales_channel_description": "C&C",
    "order_date": "2016-01-01",
    "article_id": 155487,
    "article_name": "1kg BIRNEN WILLIAMS",
    "amount": 1,
    "turnover_inc_vat": 1.85
  },
  {
    "id": 3,
    "customer_id": "Client_0146",
    "invoice_id": "Invoice_070787",
    "home_store_id": 403,
    "sales_channel_id": 1,
    "sales_channel_description": "C&C",
    "order_date": "2016-01-01",
    "article_id": 155487,
    "article_name": "1kg BIRNEN WILLIAMS",
    "amount": 8,
    "turnover_inc_vat": 1.85
  }
]
````


## Examples
This is simple example to get the sales order items of a specific item using python3 as a programming language

```python
import requests
import json

url = "http://metro.food-hacks.de/rpc/login"
payload = "{ \"email\": \"metro@foodhacks.de\", \"pass\": \"NOT_PUBLIC_YET\"}"
headers = {
    'content-type': "application/json"    
    }

response = requests.request("POST", url, data=payload, headers=headers)

# get response json
login_response = json.loads(response.text)

# get jwt token
jwt_token = login_response['token']

# GET Sales orders of a specific date

sales_order_url = "http://metro.food-hacks.de/salesorders?order_date=eq.2016-01-01"

headers = {
    'authorization': "Bearer {}".format(jwt_token),
    'cache-control': "no-cache"
    }

response = requests.request("GET", sales_order_url, headers=headers)

sales_orders = json.loads(response.text)

sales_order = sales_orders[0]


# GET salesorder items of a specific sales order

sales_order_id = sales_order['invoice_id']


sales_order_items_url = "http://metro.food-hacks.de/salesorder_items"

querystring = {"invoice_id": 'eq.{}'.format(sales_order_id)}

headers = {
    'authorization': "Bearer {}".format(jwt_token),
    'cache-control': "no-cache"
    }

response = requests.request("GET", sales_order_items_url, headers=headers, params=querystring)

sales_order_items = json.loads(response.text)
```


