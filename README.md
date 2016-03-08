# SharePass

SharePass is a safe way to quickly share passwords when traditional encryption methods are unavailable. SharePass is entirely browser-based, making it compatible with any device that supports JavaScript.

## How It Works

The sharer provides a password and an email address for the recipient. SharePass uses the email address to encrypt the password on the client. The client sends the encrypted password to the server and associates it with a secure token. SharePass stores the token and the encrypted password in the database. SharePass then uses the token to create a one-time use link and present it to the sharer. If the sharer did not check the "send to recipient" box when creating the password entry, then the sharer must send the link to the recipient manually. If the sharer checked the "send to recipient" box, then SharePass will send the link to the recipient automatically. When the recipient visits the shared link, SharePass embeds the encrypted password in the resulting page and permanently removes all password related data from the database. SharePass then prompts the recipient for their email address and uses it to decrypt the encrypted password on the client. In addition to the share link's self-destruct mechanism, SharePass has a configurable time-to-live (TTL) for encrypted password data, after which SharePass destroys expired data.

## Security

SharePass leverages AES encryption from the CryptoJS JavaScript library. SharePass conducts all encryption routines on the client, so the server never sees password data in clear text. By default, SharePass does not automatically send the share link to the recipient via email, as this would require the email address on the server. Since the email address is the encryption key, the presence of the email address on the server, even though it is never stored in the database, puts the encrypted passwords at risk to anyone with access to the server itself. When used with a reasonable TTL (10 minutes) and bypassing the option to have the server send the password to the recipient, it is highly infeasible that even someone with access to the server itself could compromise shared passwords. The source code is available for audit at [https://github.com/lanmaster53/sharepass](https://github.com/lanmaster53/sharepass).

## Risk

I recommend always using something like PGP to transmit sensitive data. However, in the event that a password must be transmitted where traditional encryption methods are unavailable, then SharePass can help reduce (not eliminate) the risk. Even though SharePass implements controls to protect against caching and man-in-the-middle attacks, there are still ways that a malicious party can compromise passwords. For example, someone with access to the server and database could compromise any password shared using the "send to recipient" option. A man-in-the-middle, or someone with access to a recipient's email account, could compromise a password by capturing the shared link and email address and using them fast enough to avoid the TTL and self-destruct mechanism. SharePass is not a fully secure means for transmitting passwords. It is merely a safer way to share passwords in a restricted environment.
