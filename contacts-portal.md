## Contacts Portal

Each Piggy Client has their own Contacts Portal, which is basically a PWA (Progressive Web App) which they can use to serve their Contacts. The Contacts Portal allows you to create your own platform for your Loyalty Program. You can personalize this environment to fit the look and feel of your business.

### Get Authenticated Url

This function generates a secure, authenticated URL specifically for a given Contact, identified by their UUID. This URL grants the Contact access to the portal, ensuring a safe and personalized experience.

GET

`https://api.piggy.eu/api/v3/oauth/clients/contacts-portal/auth-url`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`contact_uuid` `string`

`REQUIRED`

The UUID of the Contact for whom the authenticated url is to be generated.

Response Example

Show more

`1
2
3
4
5
6
` `{
    "data": {
        "url": "https://custom.appurl.com/home?token=eyJpdiI6IkF3NXpyM1FGcls5VW5MQjZUR0FPMVE9PSIsInZhbHVlIjoiYUFa2HFVVFdmTmQ2aGdOcmdyQUFMTjJvSFAyS2R2RHdSZDFscC84Mnd4V0NrSTdMbTJZaDhrZUN5TENjKzJYd0ZmYXpybWwvVFVyRXlwVGRvWTdEcHc9PSIsIm1hYyI6IjkwNzdlZDU3ZjIyMGIyOGVmM2NiYzVlOWM3ZGViYWIwMjAzYjNkYTc0M2Y1NGYyYmYyZDFlMWMyZjE2MzJlOGEiLCJ0YWciOiIifQ=="
    },
    "meta": {}
}`

Possible errors

Code

Message

`1003`

Invalid input.

`55031`

Contact not found.

### Related

[**Contact Identifiers** \\
Contacts can also be found using Contact Identifiers](https://docs.piggy.eu/v3/oauth/contact-identifiers) [**Credit Receptions** \\
The issuing of credits is done using so-called Credit Receptions](https://docs.piggy.eu/v3/oauth/credit-receptions)
