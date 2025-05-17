## Subscription Types

Subscription Types are ways of setting up different categories for your communication. It's like a mailing list. By default, an Account has two Subscription Types: one for marketing purposes, and another for functional emails.

### Enrollment strategies

For each Subscription Type, an enrollment `strategy` needs to be chosen, which decides how Contacts can opt in or out of that specific type. There are three options:

- **1\. Opt-out:** New Contacts are enrolled automatically and will need to opt out to unsubscribe.

- **2\. Opt-in:** New Contacts are not enrolled automatically and will need to opt in to subscribe.

- **3\. Double opt-in:** New Contacts are not enrolled automatically and will need to opt in, and then confirm via email to subscribe.

For API purposes, when using the opt-in and opt-out strategies, Contacts can be subscribed and unsubscribed via API calls. When using the double opt-in `strategy`, Contacts can be unsubscribed via API, but they cannot be (fully) subscribed. When subscribing a Contact to a double opt-in Subscription Type via API, they will be sent an email asking for confirmation. The email that is sent can be customized in the Business Dashboard.

### List Subscription Types

Retrieves a list of all Subscription Types linked to an Account. Useful for getting an overview of the various communication categories and their enrollment strategies.

GET

`https://api.piggy.eu/api/v3/oauth/clients/subscription-types`

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
` `
    {
    "data": [\
        {\
            "name": "Monthly offers",\
            "description": "A monthly email blast with our best offers!",\
            "active": true,\
            "strategy": "DOUBLE_OPT_IN",\
            "uuid": null,\
            "id": 3\
        },\
        {\
            "name": "Marketing",\
            "description": "Subscribe here for marketing purposes",\
            "active": true,\
            "strategy": "DOUBLE_OPT_IN",\
            "uuid": "123123123",\
            "id": 1\
        },\
        {\
            "name": "Product updates",\
            "description": "Never miss out on any of our newest features",\
            "active": true,\
            "strategy": "OPT_OUT",\
            "uuid": null,\
            "id": 5\
        },\
        {\
            "name": "Events",\
            "description": "Stay in the loop for all our special events",\
            "active": true,\
            "strategy": "OPT_IN",\
            "uuid": null,\
            "id": 4\
        }\
    ],
    "meta": []
}`

### Related

[**Rewards** \\
List your rewards and update rewards](https://docs.piggy.eu/v3/oauth/rewards) [**Reward Receptions** \\
It's what Contacts are saving up for: Reward Receptions](https://docs.piggy.eu/v3/oauth/reward-receptions)
