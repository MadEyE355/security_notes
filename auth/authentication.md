# Authentication
### Verifying if a user is actually who he claims to be

## Methods
- Password
- OTP
- Federated Identity
- Passkeys

# Session Management
### HTTP is stateless so a method is required to remeber user states there are two main models of session management **Centralized** and **Decentralized**

## Centralized
### In this model the server keeps the record of user session state
- Server makes a session ID
- Server sends copy of session ID ot client while keeping a copy
- Client sends session ID with each request

### Attacks
- Intercepting the session ID (MITM attack)
- Attacking the client (XSS)
- Guessing the session id

### Values
- The session id should have enough randomness to not be guessable
- Not contain any actual info about user (be meaningless)

### Storage
There are two main ways to  ways of storing the session id __Cookies__ and __Local Storage__
- Cookies are good as they have safety flags like `Same Site` _(CSRF)_, `Secure` _(MITM)_, `HTTP only` _(XSS)_ to protect against different attacks

### Session Lifetime
- cookies attributes like `Expires` and `Max-Age` can be used to control lifetime 
- Timeouts such as __Idle__ , __Absolute__ can be used to define time period of cookie expiry
- When timer runs out server generates new cookie -> invalidates old one (optional) -> send new one to client __[Cookie Renew]__
- _Refresh Tokens can be used in this model too_

## Decentralized
### in this model user state is not kept by server instead signed objects are used such as __jwt__
- server creates a token representing users state
- digitally sign the token
- returns signed token to user

#### Since only client keeps the tokens to servers are used to process the request __Token Server__ and __Resource Server__

### Invalidating Sessions
-  Give tokens client uses to access resources _(Access Tokens)_ a shorter valid period (meaning they can be used for long)
- Add a new type of token **Refresh Token** , which client gives 
- when presented with refresh token server creates new access tokens and gives to client

# Other points
- Decentralized Session management is only prefered when working with distributed systems due to the increased complexity of token handling and invalidating
- It is harder to invalidate token in decentralized 
- By default cookies set by a site are only sent to that site and not even its subdomains , `Domain` attribute of cookies can be changed to change this behaviour 
