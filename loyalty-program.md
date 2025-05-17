## Loyalty Program

Contacts save credits at Loyalty Programs. Clients (people, store-owners or corporations who buy our software) may not use Loyalty at all, or join a Loyalty Program not belonging to them, or they may use several Loyalty Programs. The collection of credits by the end-user is Loyalty Program specific, meaning the credits saved at program A cannot be redeemed at program B. However, although a Loyalty Program belongs to one Account, that Account may choose to invite other accounts to partake in that Loyalty Program. The Loyalty Program has some global settings. Which can be retrieved with the get Loyalty Program call.

### Get Loyalty Program

This method allows you to retrieve detailed information about a specific Loyalty Program. It provides key details such as the program's ID, `name`, maximum credit amount, `custom_credit_name`, and `unit` settings. This API call is allows for understanding the details of the Loyalty Program for your Account.

GET

`https://api.piggy.eu/api/v3/oauth/clients/loyalty-program`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Response Example

Show more

`1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
` `
	{
    "data": {
        "id": 3545,
        "name": "Rens Testje",
        "max_amount": 50000,
        "custom_credit_name": "Credits",
        "unit": {
            "name": "test",
            "label": "Calories",
            "prefix": ""
        }
    },
    "meta": {}
}`
