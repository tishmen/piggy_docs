## Automations

Automations are a key feature that help our clients automate a variety of tasks. With Automations, you can send out marketing emails automatically, set up loyalty programs, issue credits, and send digital gift cards. These Automations are driven by specific events and are designed to work with individual contacts.

You can also start Automations manually or through our API for a specific contact. When using the API, you have the option to send extra data along with the Automation Run for use in that run.

### List Automations

Get an overview of all created Automations for an account.

GET

`https://api.piggy.eu/api/v3/oauth/clients/automations`

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
` `
    {
    "data": [\
        {\
            "event": "physical_reward_reception_created",\
            "name": "Send Reward",\
            "status": "inactive",\
            "created_at": "2022-05-10T12:47:38+00:00",\
            "updated_at": "2022-05-10T12:47:38+00:00"\
        },\
        {\
            "event": "contact_updated",\
            "name": "Send email on birthday",\
            "status": "active",\
            "created_at": "2022-05-11T08:09:37+00:00",\
            "updated_at": "2022-05-11T08:09:37+00:00"\
        },\
        {\
            "event": "credit_reception_created",\
            "name": "Send mail on first credit reception",\
            "status": "active",\
            "created_at": "2022-05-17T10:28:11+00:00",\
            "updated_at": "2022-05-17T10:28:11+00:00"\
        },\
    ],
    "meta": []
}`

### Create Automation Run

This method triggers an Automation. Please note that only the Automation with the 'contact\_updated' event can subsequently be used to create a run. This means that the `event` of the Automation that you provide should have a be of type 'contact\_updated'. You can add an array at `data`, which can then be used in the Automation.

POST

`https://api.piggy.eu/api/v3/oauth/clients/automations/runs`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`contact_uuid` `string`

`REQUIRED`

Contact for whom the Automation is to be triggered.

`automation_uuid` `string`

`REQUIRED`

Automation to be triggered.

`data` `array`

`OPTIONAL`

Data to be used in the Automation.

Response Example

Show more

`1
2
3
4
` `{
    "data": [],
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

`55031`

Contact not found.

`60013`

Model not found.

### Related

[**Contact Identifiers** \\
Contacts can also be found using Contact Identifiers](https://docs.piggy.eu/v3/oauth/contact-identifiers) [**Credit Receptions** \\
The issuing of credits is done using so-called Credit Receptions](https://docs.piggy.eu/v3/oauth/credit-receptions)
