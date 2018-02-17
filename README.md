# passport-oauth2-client-nosecret

Just like [passport-oauth2-client-password](https://github.com/jaredhanson/passport-oauth2-client-password), except it doesn't require the `client_secret`.

## Notes

Only unprotected clients like native and single-page applications should use this strategy. Additionally, developers must implement additional security enhancements (e.g., [PKCE](https://tools.ietf.org/html/rfc7636)) if necessary.

## Installation

```
npm install passport-oauth2-client-nosecret --save
```

## Usage

```
passport.use(new ClientNoSecretStrategy(function(client_id, client_secret, next){
    Client.findOne({ client_id: client_id }, function(err, client) {
        if(err) return next(err);
        
        if(!client) return next(null, false);
        
        if(client_secret && client.secret !== client_secret) return next(null, false);

        return next(null, client);
    });
}));

app.post(
    '/endpoint',
    passport.authenticate(['oauth2-client-nosecret'], { session: false }),
    oauth2orize.token()
);
```