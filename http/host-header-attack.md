# Host Header Attack
#### Host headers are headers sent by the client to the server for servers to understand where the request actually belongs to (it is necessary to put host headers in HTTP 1.1)

## Why are host headers needed?
### Due to increasing use of cloud based solutions and ipv4 address being exhausted web servers have started to host more than 1 website or application today which was not the case in ealier times when one server only hosted one website.

### This has created the need of using host headers so servers can manage multiple websites and know which site the user has requested.

## Problem (Host header attack)
### Host header are sent in the actual http request looking like this

```http
GET /some_data HTTP/1.1
Host : good_site.net
```
### As the request host is actually defined in the http request attacker can change it and could cause issues if site uses it to generate urls.




## Common exploitation techniques
### 1. simply changing the host header
```http
GET /some_data HTTP/1.1
Host : evil_site.net
```
#### Here evil_site.net is controlled by attacker , if a site has no protection and allow any host header this could work

### 2. Multiple header
```http
GET /some_data HTTP/1.1
Host : good_site.net
Host : evil_site.net
```
#### Supplying multiple headers could cause disagrement on the real host header causing the evil header to pass through.

### 3. Indenting
```http
GET /some_data HTTP/1.1
    Host : some_site.net
Host : evil_site.net
```
#### Doing this type of indentation could make the server count one header as part of the previous part and could make the evil host work

## Exploting Host header attack
#### Here are some exploitation that could done

## 1. Password reset poisoning
### If the attacker is able to change to change host headers to his website he could set up a website (example : evil_site.com) and could click forget password of some other user (victim)

### many site use a temporary token to reset password of user , when they request password reset a link including this token is sent to there mail 

### Example
```http
GET /reset-pass HTTP/1.1
HOST : good_site.com
```
#### this could generate a link which would look something like 
> `good_site.com/reset/tokenid=12345`

### the attacker could change the host header if the site uses this type of urls and make the link go to his site (evil_site.com)

> attacker --> types user name of victim --> clicks forget password --> changes host header to his site --> forwards the request


### Host header changed by attacker before forwarding request
```http
GET /reset-pass HTTP/1.1
HOST : evil_site.com
```

### Now the password link would look something like this
> `evil_site.com/reset/tokenid=12345`

### Instead of taking victim to good_site it takes it to bad_site which is controlled by attacker and now the attacker steals the tokenid of victim as he can just see the logs on his website which will show that victim visited `evil_site.com/reset/tokenid=12345`

### The attacker than just use this token *(visit the actual site and use this token here he it would be `good_site.com/reset/tokenid=12345`)* and change victims passowrd controlling his account,

>**NOTE** : good_site and evil_site are used just to give reference in reailty the attacker would always use something which looks like the actual website , example here if the actual site is `good_site.com` the attacker would use something like `good-site.com` instead of `evil-site.com`

## How to prevent
### 1. Dont allow using any value in place of host set values which will are allowed and only accept request if host header is one of those values

### 2. Try not to use headers like `X-Forwarded-Host` and if using only allow some trusted headers to be in them instead of allowing any header
