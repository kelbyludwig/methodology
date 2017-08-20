# OAuth2.0 Attack Surface

## Threat: CSRF against the `redirect_uri`

### About

CSRF against the `redirect_uri` can enable "login CSRF". For example, an attack
can send a victim a link with an authorization code that is bound to his
account. Dependent on the OAuth implementation, this could enable a link
between the victim and the attacker.

* Risk: High->Low

### Mitigation

Use and validate the OAuth2.0 `state` parameter. An authorization code should
be bound to a user-agent's authenticated session.

Additionally, ensure that the state validation does not allow for user-supplied
`state` or empty `state` values.

### References

* [OAuth2.0 Specification on CSRF](https://tools.ietf.org/html/rfc6749#section-10.12)

* [The Most Common OAuth2.0 Vulnerability](https://homakov.blogspot.com/2012/07/saferweb-most-common-oauth2.html)

## Threat: CSRF against the authorization endpoint

CSRF against the authorization endpoint could allow an attacker to authorize an
application for the victim's account without the victim's direct consent. I
believe this may require other shortcomings (or only be exploitable in some
configurations), as the direct response to CSRF won't usually be processed by
the browser (I'm sure there are some cases where this is not true).

* Risk: High->Info

### Mitigation

Ensure the authorization endpoint has CSRF protection.

### References

* [CSRF and OAuth2.0](https://spring.io/blog/2011/11/30/cross-site-request-forgery-and-oauth2)

## Threat: Leaking `authorization_code` in the `code` grant type

### About 

* This can be done in multiple ways:

    * A malicious, yet valid `redirect_uri` that points to an attacker
    controlled domain

    * A malicious, yet valid `redirect_uri` with an open redirect
    vulnerability can leak code's in Referer headers

    * A malicious, yet valid `redirect_uri` that has content that generates
    attacker-controlled requests (e.g. images on a facebook wall) can leak
    code's in Referer headers

* Risk: High->Medium

### Mitigation

`redirect_uri`'s should be strictly validated. Ideally, strict string
comparison should be used (e.g. subdomains, additional parameters, etc. should
not be allowed)

### References

* None

## Threat: Using OAuth for AuthN

### About 

This is often a mis-application of OAuth. Consider the following:

    * Eve authorizes a third-party application (`tweet-reader.com`) to
    access her tweets and her Twitter profile information.

    * Due to the previous authorization, `tweet-reader.com` has an access
    token for Eve.

    * The admin of `tweet-reader.com` goes rogue and decides to use Eve's
    access token for malicious purposes.

    * Another third-party service that Eve uses e.g.
    `private-messaging-app.com` (mis-)uses OAuth for authentication.

    * To authenticate a user, `private-messaging-app.com` uses a user's
    access token to request their Twitter profile information (e.g. a
    user's email address).

    * This means, that `tweet-reader.com` can use Eve's access token to
    authenticate *as Eve* on `private-messaging-app.com`

* Risk: Low->Info

### Mitigation

Use OIDC for AuthN.

### References

* [The Problem with Using OAuth For Authentication](http://www.thread-safe.com/2012/01/problem-with-oauth-for-authentication.html)

* [One AccessToken To Rule Them All](https://homakov.blogspot.com/2012/08/oauth2-one-accesstoken-to-rule-them-all.html)

## Threat: `response_type=token` (i.e. the "Implicit" grant type)

### About

The Implicit grant type is not necessarily vulnerable by itself. It can,
however, increase the impact of other issues vulnerabilities due to
user-agent exposure of an `access_token`

* Risk: Low->Info

### Mitigation

If possible, do not allow `response_type=token`.

### References

* [Differences between authorization codes and access tokens](https://stackoverflow.com/questions/8666316/facebook-oauth-2-0-code-and-token)

## Threat: Login CSRF

### About 

If an attacker can force a user to authN to their account ("Login CSRF"),
this may have interesting side-effects on some OAuth flows.

* Risk: Low->Info

### Mitigation

Use CSRF protection for authN requests. Also consider how "Login CSRF" could
affect your OAuth implementation and apply additional compensating controls, if
necessary.

### References
    
* [Two WONTFIX Vulnerabilities in Facebook Connect](https://homakov.blogspot.com/2014/01/two-severe-wontfix-vulnerabilities-in.html)

## Threat: Authorization Code Re-Use

### About

Authorization codes could be somehow compromised sometime after legitimate use.
Ideally, this would not enable the attacker who compromised these codes to
collect access tokens.

* Risk: Low->Info

### Mitigation

Avoid storing `authorization_code`s long-term. Ideally, design them where they
are one-time-use (as the specification suggests)

### References

* [Security Considerations: Authorization Codes](https://tools.ietf.org/html/rfc6749#section-10.5)
