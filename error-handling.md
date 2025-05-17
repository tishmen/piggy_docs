## Error handling

Requests that result in an error always have the same response syntax:

`1
2
3
4
5
` `{
      "message": "<message>",
      "code": <code>,
      "status_code": 400
    }`

Error responses have a status code 400 and are accompanied by a declarative message. Underneath each of the available API calls, we've listed the errors you may encounter in that specific API call. However, you can also find the full list below. Hopefully the messages themselves are sufficiently clear. If you encounter an error and the error message is unclear or missing, please contact us.

Possible errors

Code

Message

`7002`

Account inactive.

`55018`

Loyalty Program not active.

`1102`

Shop not linked to a Loyalty Program.

`1103`

Loyalty Program not found.

`1003`

Invalid input.

`55012`

Member not found.

`55031`

Contact not found.

`60016`

Contact not anonymous.

`60005`

Contact already exists.

`55007`

Loyalty card not found.

`55032`

User app not found.

`1013`

Card not registered.

`60001`

Credit reception creating in progress.

`1015`

Maximum amount of credits reached.

`1048`

This card was not found

`6003`

Reward not found.

`60007`

Unit not found.

`55016`

The card is already linked to a member.

`55046`

No default unit found.

`55017`

Member already exists.

`1017`

Onvoldoende punten \| Insufficient credits.

`1027`

POS Transaction not found.

`6001`

The POS transaction is already linked to a credit reception.

`1019`

You have reached your claim limit on this reward.

`1018`

The stock on this reward is empty.

`6007`

The linked template does not contain the right replacement variable.

`1041`

Staged Credit Reception not found.

`1034`

Staged credit reception is already redeemed.

`1008`

Shop not found.

`7011`

User has no access to this account.

`5001`

Giftcard not found.

`7010`

That giftcard is not valid.

`5003`

Het bedrag op de kaart is niet toereikend. \| Insufficient amount on giftcard.

`5007`

Deze giftcard is niet gekoppeld aan een programma. \| Giftcard not linked to Giftcard Program.

`7015`

Shop does not have access to Giftcard Program.

`5006`

Deze giftcard kan niet opgewaardeerd worden. \| This giftcard cannot be incremented.

`1114`

Only non re-incrementable giftcards can be used in the OAuth Clients API.

`5008`

Maximale limiet op kaart bereikt. \| Max amount will be exceeded.

`5002`

Het bedrag op de kaart is verlopen. \| This giftcard has expired.

`55004`

Marketing program not found.

`55020`

Marketing recipient not found.

`55005`

Marketing program customer already exists.

`55021`

Only Digital giftcards can by created using the public API.

`1028`

Credit reception not found.

`55024`

This credit reception was not created by this shop.

`1010`

Kaart is niet geactiveerd. \| Card is not activated.

`55025`

This is not the giftcards last transaction.

`60003`

Contact identifier not found.

`60004`

Contact identifier inactive.

`60006`

Contact identifier already exists.

`60014`

Contact identifier is already linked.

`60011`

Contact identifier is not linked to a contact.

`60009`

Attribute already exists.

`55030`

Subscription type not found.

`13010`

Passcode not found.

`55045`

Unit already exists.

`60013`

Model not found.

`60018`

Please submit credits when using v1

`60019`

Please submit units when using v2

`60000`

Usage limit reached.

### Related

[**Entities** \\
Get to know our entities and their relations](https://docs.piggy.eu/v3/entities) [**OAuth or Register API** \\
Piggy Public API has two forms of authentication, learn which one to use](https://docs.piggy.eu/v3/guides/oauth-or-register)
