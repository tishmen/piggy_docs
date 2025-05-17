## Authorization

**Note:** Personal Access Tokens from **Register** objects cannot be used. They are for Register API routes only.

### Personal Access Tokens

On our OAuth API routes, the use of Personal Access Tokens is now the preferred method of authorization.

In the Business Dashboard, Personal Access Tokens can be created under 'Apps', then 'Integrations', then ['Personal Access Tokens'](https://docs.piggy.eu/v3https:/business.piggy.eu/apps/integrations/personal-access-tokens). Simply create a new one if none exists, and the Personal Access Token will be provided for you. **Note:** the token will only be shown once, so make sure to copy it down.

As with the Register API and the previously used OAuth Clients, the header format for all subsequent API calls is:

`Authorization: Bearer {{ Personal Access Token }}`

#### Expiration

Upon creation of the Personal Access Token, an expiration date of your choosing can be set.

Deprecated: OAuth Clients

Note: Deprecated

The Client ID and Client Secret from the Client can be used to initiate the OAuth handshake between Piggy and your integration.

#### Requesting an Access Token

To request an access token, perform a call to https://api.piggy.eu/oauth/token when on the Production environment, with the following payload:

`1
2
3
4
5
` `{
        "grant_type": "client_credentials",
        "client_id": "your-client-id",
        "client_secret": "your-client-secret"
}`

The returned access\_token will be the token that you need to add to your subsequent requests.

#### Using OAuth 2.0 Access Tokens

OAuth 2.0 access tokens are provided as a bearer token in the Authorization http header.

The header format is:

`Authorization: Bearer {{ access_token }}`

#### Manage Token Expiration

Your application needs to manage access token expiration. Access tokens offered with Client Credentials Grant expire in certain time. Also, the authorization server does not support refresh requests. So your application needs to obtain a new access token before or after expiration. You can use one of the two methods below:

1. **Calculate expiration time:** Use the expires\_in field of token response. You can calculate expiration time by using the field which represents the lifetime of the access token and the time you have received it. Let your application calculate and manage the expiration time of access token so that it can make a token request before the token expires.
2. **Detect invalid\_token error:** Utilize invalid\_token error responses. Piggy verifies each access token and will return an invalid\_token error response (with status code 401) when the access token has expired. So, when your application detects the error, let it make a token request to obtain a new access token.

### Related

[**Contacts** \\
Contacts are the basis of the loyalty system, for whom any action is created](https://docs.piggy.eu/v3/oauth/contacts) [**Credit Receptions** \\
The issuing of credits is done using so-called Credit Receptions](https://docs.piggy.eu/v3/oauth/credit-receptions)
