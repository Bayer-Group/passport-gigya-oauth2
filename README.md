# passport-gigya-oauth2

[Gigya](https://www.gigya.com/) OAuth 2.0 authentication strategy for [Passport](http://passportjs.org/).

This module lets you authenticate using Gigya in your Node.js applications.
By plugging into Passport, Gigya authentication can be easily and
unobtrusively integrated into any application or framework that supports
[Connect](http://www.senchalabs.org/connect/)-style middleware, including
[Express](http://expressjs.com/).

## Install

    $ npm install passport-gigya-oauth2

## Usage

#### Configure Strategy

The Gigya authentication strategy authenticates users using a Gigya
account and OAuth 2.0 tokens.  Gigya's OAuth 2.0 endpoints, as well as
the client identifer and secret, are specified as options.  The strategy
requires a `verify` callback, which receives an access token and profile,
and calls `done` providing a user.

    passport.use(new OAuth2Strategy({
        authorizationURL: 'https://www.example.com/as/authorization.oauth2',
        tokenURL: 'https://www.example.com/as/token.oauth2',
        clientID: EXAMPLE_CLIENT_ID,
        clientSecret: EXAMPLE_CLIENT_SECRET,
        callbackURL: "http://localhost:3000/auth/oauth2/callback'
      },
      function(accessToken, refreshToken, profile, done) {
        User.findOne({ 'gigya.id' : profile.id }, function (err, user) {
            if (err) { return done(err); }
            if (user) {
                return done(null, user);
            } else {
                var newUser = new User();
                newUser.gigya.id    = profile.id;
                newUser.gigya.token = accessToken;
                newUser.gigya.name  = profile.displayName;
                newUser.gigya.email = profile.email;
                newUser.save(function(err) {
                    if (err) { throw err; }
                    return done(null, newUser);
                });
            }
        });
      }
    ));

#### Authenticate Requests

Use `passport.authenticate()`, specifying the `'oauth2'` strategy, to
authenticate requests.

For example, as route middleware in an [Express](http://expressjs.com/)
application:

    app.get('/auth/oauth2',
        passport.authenticate('oauth2', { scope : ['openid', 'profile', 'email'] }));

    app.get('/auth/oauth2/callback',
        passport.authenticate('oauth2', { failureRedirect: '/login' }),
        function(req, res) {
            // Successful authentication, redirect home.
            res.redirect('/');
        });

## Related Modules

- [passport-oauth2](https://github.com/jaredhanson/passport-oauth2) — OAuth 2.0 authentication strategy

## Tests

    $ npm install
    $ npm test

## Credits

  - [Brenden McKamey](http://github.com/bsmckamey)

## License

[The MIT License](http://opensource.org/licenses/MIT)
