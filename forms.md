## Forms

Forms are a simple yet powerful tool for collecting valuable information about your customers or employees. They allow you to systematically gather data, which can be easily transformed into meaningful business insights. Whether you're looking to understand customer spending habits or expand profiles, Forms offer the flexibility to tailor questions to your specific needs.

### List Forms

Retrieve of all Forms linked to an Account. It's possible to filter both on `status` and `type`

GET

`https://api.piggy.eu/api/v3/oauth/clients/forms`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`status` `string`

`OPTIONAL`

Status of Forms to be retrieved, possible values: PUBLISHED, DRAFT, BIN.

`type` `string`

`OPTIONAL`

Type of Forms to be retrieved, possible values: PUBLIC, PRIVATE.

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
` `{
    "data": [\
        {\
            "uuid": "a4fb2960-ef15-43da-96e0-18aa83861ace",\
            "name": "mikes",\
            "status": "PUBLISHED",\
            "type": "PUBLIC",\
            "url": "http://piggy.eu/forms/a4fb2960-ef15-43da-96e0-18aa83861ace"\
        }\
    ],
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

### Related

[**Contact Identifiers** \\
Contacts can also be found using Contact Identifiers](https://docs.piggy.eu/v3/oauth/contact-identifiers) [**Credit Receptions** \\
The issuing of credits is done using so-called Credit Receptions](https://docs.piggy.eu/v3/oauth/credit-receptions)
