## Contact Verification

If you already have a verification method in your own application – either through login, a saved uuid, magic link etc. - we recommend you use that one instead.

Business owners use the Piggy system to send out a verification email to their Contacts whenever a Contact wants to login to the business owner's Contacts Portal. This verification is configured in the Business Dashboard and sends out a code, which the Contact in question needs to fill in.

For integrations lacking a method to authenticate email addresses or Contacts, the API provides a solution for sending verification emails and validating the received code.

### Send Contact Verification Email

This function triggers the sending of a verification email to a Contact, provided they exist in the system. Therefore, it requires the Contact's email address to proceed. When doing so, the process of verifying their identity through email validation is initiated.

The system checks for the existence of the Contact and the associated Contacts Portal before sending the email. If the Contact is not found or if the Contacts Portal is missing, the process will not proceed.

POST

`https://api.piggy.eu/api/v3/oauth/clients/contact-verification/send`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`email` `string`

`REQUIRED`

The email address of the Contact to receive the email.

Response Example

Show more

`1
2
3
4
5
6
7
` `{
    "data": {
        "type": "Success",
        "message": "An email containing a link to verify your email has been sent to john@doe.com. Please check your spam / junk as well."
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

`55031`

Contact not found.

`55032`

User app not found.

### Verify Login Code

Validates the login code received by the Contact via email. This verification is a crucial step to ensure the Contact's identity and secure login. The function checks for the presence of the Contacts Portal and uses the provided code and email to complete the verification process.

POST

`https://api.piggy.eu/api/v3/oauth/clients/contact-verification/verify`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`code` `string`

`REQUIRED`

The code sent by email.

`email` `string`

`REQUIRED`

The Contact's email.

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
        "uuid": "123abc-lmnopq-3242a-8899aa"
    },
    "meta": {}
}`

Possible errors

Code

Message

`1003`

Invalid input.

`55032`

User app not found.

`13010`

Passcode not found.

### Get Auth Token

Retrieves an authentication token for a specific Contact, identified by their UUID. Allowing verification of the Contact's identity and granting secure access.

GET

`https://api.piggy.eu/api/v3/oauth/clients/contact-verification/auth-token/{{contact_uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`contact_uuid` `string`

`REQUIRED`

The Contact's UUID.

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
        "auth_token": "eyJpdiI6IkluSk9sRzNBRFFFOUFNNUh0eDZTWGc9PSIsInZhbHVlIjoiN0Nna05yZFFEbE16YjY0ME55dWcvTDZVVUl5WGdVN2cxRDJjcjl6K1lzUmxoWlYxZUVXMCt3TCtRUmNQc0g1SG9YR2dnd0wwM1N2RE54WDZhcnR2Z0FxMFZkRXNoUXN3eDN3OC9RZmgreEJ1clFyOS82eUhBeUZVWGNDdytVd2tRM01lWVJPZi90MTZkT3hXRXQwQXNnPT0iLCJtYWMiOiIyNmFlYzA0YzJjMDI3NGM4MDI2YzMyMTAzYzJmZmQ3YWEzY2VhNWZjMDgxYzQ3NGYwNThjZWY1MTJlZTdmNmVlIiwidGFnIjoiIn0="
    },
    "meta": []
}`

Possible errors

Code

Message

`55031`

Contact not found.

`1053`

Account secret is invalid

`3000`

The token is expired

### Related

[**Contacts** \\
Contacts are the basis of the loyalty system, for whom any action is created](https://docs.piggy.eu/v3/oauth/contacts) [**Rewards** \\
List your rewards and update rewards](https://docs.piggy.eu/v3/oauth/rewards)
