## Loyalty Transaction Attributes

Loyalty Transaction Attributes are Custom Attributes that you can link to your Loyalty Transactions, tailored to your specific needs. This functionality offers flexibility in collecting and organizing transaction-related data.

### List Loyalty Transaction Attributes

Retrieve a list of all Loyalty Transaction Attributes associated with your Loyalty Program. It's a helpful tool to view both standard and Custom Attributes, allowing for better understanding and management of the data collected through loyalty transactions.

GET

`https://api.piggy.eu/api/v3/oauth/clients/loyalty-transaction-attributes`

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
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
` `{
    "data": [\
        {\
            "name": "plu",\
            "label": "PLU",\
            "placeholder": null,\
            "type": "text",\
            "field_type": "text",\
            "options": [],\
            "is_piggy_defined": false,\
            "is_soft_read_only": false,\
            "is_hard_read_only": false\
        },\
        {\
            "name": "category",\
            "label": "Category",\
            "placeholder": null,\
            "type": "text",\
            "field_type": "text",\
            "options": [],\
            "is_piggy_defined": false,\
            "is_soft_read_only": false,\
            "is_hard_read_only": false\
        },\
        {\
            "name": "server_name",\
            "label": "Served by",\
            "placeholder": null,\
            "type": "text",\
            "field_type": "text",\
            "options": [],\
            "is_piggy_defined": false,\
            "is_soft_read_only": false,\
            "is_hard_read_only": false\
        },\
    ],
    "meta": {}
}`

### Create Loyalty Transaction Attribute

Creating a Loyalty Transaction Attribute allows you to add new Custom Attributes to your Loyalty Program. This feature is essential for tailoring the program to specific data collection needs, such as capturing unique customer preferences or transaction details. The creation process involves specifying the attribute's `name`, `data_type`, and other relevant details, enabling more personalized and effective data and loyalty management.

POST

`https://api.piggy.eu/api/v3/oauth/clients/loyalty-transaction-attributes`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`name` `string`

`REQUIRED`

The unique value for the Loyalty Transaction Attribute, used for internal purposes.

`data_type` `string`

`REQUIRED`

The data type of the attribute, possible options: url, text, date, phone, float, color, email, number, select, boolean, rich\_text, date\_time, long\_text, date\_range, time\_range, identifier, birth\_date, file\_upload, media\_upload, multi\_select or license\_plate.

`label` `string`

`OPTIONAL`

The label for the attribute, which is outputted.

`description` `string`

`OPTIONAL`

A description for the attribute.

`options` `array`

`OPTIONAL`

Applicable for (multi)selects only. Array consisting of name and value arrays.

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
16
17
18
` `{
    "data": {
        "name": "favorite_color",
        "label": "Favorite Color",
        "placeholder": "",
        "type": "select",
        "field_type": "select",
        "options": {
            "blue": "Blue",
            "red": "Red",
            "yellow": "Yellow"
        },
        "is_piggy_defined": false,
        "is_soft_read_only": false,
        "is_hard_read_only": false
    },
    "meta": {}
}`

Possible errors

Code

Message

`60009`

Attribute already exists.

### Related

[**Contacts** \\
Contacts are the basis of the loyalty system, for whom any action is created](https://docs.piggy.eu/v3/oauth/contacts) [**Credit Receptions** \\
The issuing of credits is done using so-called Credit Receptions](https://docs.piggy.eu/v3/oauth/credit-receptions)
