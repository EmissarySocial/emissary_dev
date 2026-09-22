
Emissary uses **JWT tokens** to support single sign on from external services.  This is an industry standard technology, with [official specifications from the IETF](https://datatracker.ietf.org/doc/html/rfc7519)

To set up single-sign-on (SSO) on your domain, first sign in to your Emissary database as a domain administrator, then go to **Server \> External \> Single Sign On**.  

## 1. Configure Shared Secrets
In the Emissary setup, you'll need to enter a secret key.  This secret will be used to authenticate sign-in requests, and should be shared only with the external system that will sign users into Emissary.

Your shared secret should be a 256-bit string with enough entropy to be un-guessable.  With SSO activated, this becomes the master key to your Emissary server.

## 2. Create a JWT Token

JWT is a secure industry standard for passing authentication tokens between servers.  Visit [JWT.io](https://jwt.io/introduction) for guides to using JWT, along with a list of pre-packaged libraries that create JWT tokens in most computer languages.

The tokens you create MUST include certain claims to be accepted by the Emissary server.

| Claims     | Description                                                                                                                                                                                                                                                                         | Required? |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- |
| `username` | The username of the person who is signing in.  This username must already exist in the target Emissary database.                                                                                                                                                                    | REQUIRED  |
| `exp`      | [Expiration date](https://datatracker.ietf.org/doc/html/rfc7519#section-4.1.4) of the token represented as the number of seconds since 1970-01-01. Though not required, it is strongly encouraged that you use a short value of 5-10 minutes to maintain security of user accounts. | OPTIONAL  |

### Example Token

Here is an example JWT token that you can inspect with the [JWT.io debugger](https://jwt.io?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InNhcmFoLWNvbm5vciIsImV4cCI6MTcxNzQ1OTIwMH0.kxAfOq1mng-T_k4VbYh4iiKnznA5lzJHyhICdd_7MOk) that signs in with the username `sarah-connor`.  This token expires June 4, 2024, and was created with the signing key `1234567890`.

## 3. Sign In To Emissary

To sign in to Emissary from your remote system, simply link or forward your user to a URL with the following format

`https://<YOUR-SERVER-NAME>/.sso?token=<YOUR-JWT-TOKEN>`

Emissary will authenticate the token, then sign the user in to their profile page.  Done!

## Questions? Comments?

The goal of this SSO mechanism is to be simple to implement, yet cryptographically secure.  Please reach out directly to [emissary@emissary.social](mailto:emissary@emissary.social) if you have questions or enhancements to this process.
