### Find Contact Session Token

Find a Contact's Session Token to identify and validate them.

GET

`https://api.piggy.eu/api/v3/oauth/clients/contact-session-tokens/{{contact_session_token}}`

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
` `{
    "data": {
        "token": "12345abcdefg",
        "contact": {
            "uuid": "123abc-def789-qwerty-34567"
            "email": "john@doe.com
        },
        "status": "active",
    },
    "meta": []
}`

### Use Contact Session Token

Mark a session token as used, making it invalid for next attempts.

POST

`https://api.piggy.eu/api/v3/oauth/clients/contact-session-tokens/{{contact_session_token}}/use`

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
` `{
    "data": {
        "token": "12345abcdefg",
        "contact": {
            "uuid": "123abc-def789-qwerty-34567",
            "email": "john@doe.com
        },
        "status": "used",
    },
    "meta": []
}`

### Related

[**Contacts** \\
Contacts are the basis of the loyalty system, for whom any action is created](https://docs.piggy.eu/v3/oauth/contacts) [**Rewards** \\
List your rewards and update rewards](https://docs.piggy.eu/v3/oauth/rewards)
