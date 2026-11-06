# IP Address Redaction (server-side GTM)

A variable template for **server-side** Google Tag Manager that returns the visitor's IP address with the last part removed, using Google's own `getAnonymizedIpAddress` API.

- **IPv4:** the last octet becomes 0, so `198.51.100.42` becomes `198.51.100.0`.
- **IPv6:** only the first 48 bits are kept.
- **No address:** if the address can't be read, it returns `::`.

## Why

Google has told advertisers that from 11 January 2027 its ad tags in a server container (Google Ads, Display & Video 360, Search Ads 360 and Campaign Manager 360) receive the full IP address unless you redact it. GA4 tags keep redacting it. An IP address is personal data under UK and EU GDPR, so it needs a lawful basis and, in practice, consent. More in [Server-side GTM will send full IP addresses to Google Ads](https://www.metricowl.co.uk/article/server-side-gtm-full-ip-google-ads-2027).

## Set up

1. In your server container, go to **Templates**, then **Search Gallery** (or **New**, then the three dots, then **Import**, and choose `template.tpl`).
2. Create a variable from the template, for example **IP Address Redaction**.
3. Go to **Transformations**, then **New**, and choose **Augment event**.
4. Add the parameter `ip_override` with the value `{{IP Address Redaction}}`.
5. Under **Affected tags**, choose the Google Ads and other advertising tags.
6. Test in Preview: on the tag's outgoing request, the IP address should end in `.0`.

Some third-party tags read the IP address from a request header rather than `ip_override`, so check each one.

## Settings

| Setting | What it does |
|---|---|
| When to redact | **Always** (default), or **unless advertising consent is granted** |
| Advertising consent | Only for the second option. A variable that returns `granted`, `true` or `1` when the visitor has granted advertising consent. Anything else, including an empty value, redacts. |

With consent granted, the variable returns the `ip_override` value the client set. If there isn't one, it redacts anyway.

## Permissions

- **Reads the visitor's IP address** (`user_ip`), for `getAnonymizedIpAddress`.
- **Reads the event data key `ip_override`**, only for the consent option.

## Licence

Apache License 2.0. See [LICENSE](LICENSE).

Built and maintained by Marc Alexander at [Metric Owl](https://www.metricowl.co.uk), a GA4 and Google Tag Manager consultancy in London. Issues and suggestions are welcome.
