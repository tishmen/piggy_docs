## Brand Kit

A Brand kit expresses the identity of your company. A brand kit is a collection of designs and features that shape the personality and look of your company. Think about your logo, colors used and font. You can also add links to your social media accounts in emails and custom app through the Brand Kit.

### Get Brand Kit

Retrieve the details of your account's Brand Kit with this call. It allows you to access all the branding elements associated with your account, including logos, color schemes, and font styles.

GET

`https://api.piggy.eu/api/v3/oauth/clients/brand-kit`

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
` `{
    "data": {
        "small_logo_url": "https://images.google.com/photo_small",
        "large_logo_url": "https://images.google.com/photo_large",
        "primary_color": "#ad5959",
        "secondary_color": "#c8bcbc",
        "tertiary_color": "#a72f2f",
        "quarternary_color": "#317f88",
        "font_family": "Arial"
    },
    "meta": []
}`

### Related

[**Contact Identifiers** \\
Contacts can also be found using Contact Identifiers](https://docs.piggy.eu/v3/oauth/contact-identifiers) [**Credit Receptions** \\
The issuing of credits is done using so-called Credit Receptions](https://docs.piggy.eu/v3/oauth/credit-receptions)
