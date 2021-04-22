# Universal Deep Link (UDL) Server

This is a server that bounces traffic to better leverage Deep Linking in mobile apps.

The project's objectives are to be a simple, effective and lightweight tool that can help any website provide a seamless integration with their mobile apps.

The server is hosted at [`https://forem-udl-server.herokuapp.com`](https://forem-udl-server.herokuapp.com) for use for Forem's mobile app.

---

**The rest of this document come directly from the [UDL-Server project readme](https://github.com/fdoxyz/udl-server/blob/main/README.md)**

---
## How it works, and why?

It's a dead simple pivot server that will redirect to whatever you pass in the `r` query param to the root path.

Modern mobile browsers provide developers with [Universal Links (iOS)](https://developer.apple.com/library/archive/documentation/General/Conceptual/AppSearch/UniversalLinks.html) or [Android Intents](https://developer.chrome.com/docs/multidevice/android/intents/) to support deep linking users from a website directly into a mobile app. However, Operating Systems currently won't trigger these features when the user clicks a link within the same domain or when the user types the URL directly in the address bar.

These and other edge cases make for a less than ideal experience, if your objective is to allow for a seamlessly transition to your mobile app. A custom banner (in your website) that links to an external site will trigger the deep linking though, and this is where the UDL Server comes in.

![diagram](udl-server-diagram.png)

## Self-hosting

Power users will likely need better reliability and scalability than a free service is able to offer. Self-hosting with Heroku (or similar SaaS platforms) is as easy as:

1. Fork this repository
1. Configure the app to automatically deploy to your Heroku account
   - Using a [custom domain with Heroku](https://devcenter.heroku.com/articles/custom-domains) is very simple (i.e. `udl.your-domain.com`)
   - Heroku's default subdomain works too (i.e. `my-app.herokuapp.com`)
1. Keep up with upstream (this repo) for future updates
   - `git remote add upstream git@github.com:fdoxyz/udl-server.git`
   - `git pull upstream main`
   - `git push origin main`

## Throttling, Safelist and Blocklist

The UDL Server uses [Rack::Attack](https://github.com/rack/rack-attack) to protect itself against abuse. It will respond with a `429` instead of the expected redirect when this happens.

[IP based throttling](https://github.com/rack/rack-attack#throttling) is enabled by default with a limit of 3 requests on a 10 second period, but only if you provide access to a Redis to work as cache (via `REDIS_URL` ENV variable). You can override these values by using `UDL_THROTTLE_LIMIT` and `UDL_THROTTLE_PERIOD`.

You can further restrict if the server will allow or deny a redirect based on passing in a regular expression via `UDL_SAFELIST_REGEXP` or `UDL_BLOCKLIST_REGEXP`. These regular expressions will be checked against the `r` param and will allow or deny the response (respectively). For example:

```bash
# All redirect requests for "tiktok.com" will be safelisted
# https://github.com/rack/rack-attack#safelisting
UDL_SAFELIST_REGEXP="/^https:\/\/tiktok.com/"
```

[Read more](https://github.com/rack/rack-attack#how-it-works) about how `Rack::Attack` safelist/blacklist features work.

## Troubleshooting

Some common details to keep in mind in case your redirects aren't working properly:

- Make sure your redirects are all using `https`
- You will likely need to make this request on a `target="_blank"` anchor tag in order to get Apple's Universal Links to work.
- Make sure your iOS app has properly configured [Associated Domains](https://developer.apple.com/documentation/safariservices/supporting_associated_domains) for the websites you want to support.
   - There's a chance it won't work in development mode (i.e. only signed with a Production certificate). I suggest releasing to TestFlight in order to properly test everything.

## Contributing

Please check out the [Contributing Guide](https://github.com/fdoxyz/udl-server/blob/main/CONTRIBUTING.md).

## Code of Conduct

This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Code of Conduct](https://github.com/fdoxyz/udl-server/blob/main/CODE_OF_CONDUCT.md).

## License

Released under an [MIT License](https://github.com/fdoxyz/udl-server/blob/main/LICENSE.txt)
