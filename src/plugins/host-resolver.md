# host-resolver

Resolve real IP addresses by querying DNS servers.

## Access Points

- `resolver`: Resolver. Resolve domain names to real IP addresses.

## Parameters

```json
{
  "udp": ["redir-8888.udp", "redir-8844.udp"],
  "tcp": ["redir-8888.tcp", "redir-8844.tcp"],
  "doh": [{
    "url": "https://1.1.1.1/dns-query",
    "next": "tls-client.tcp"
  }]
}
```

- `udp`: Descriptor of the Datagram Session Outbounds to send DNS requests.
- `tcp`: Descriptor of the Stream Outbounds to send DNS requests. *Not used as of YtFlowCore 0.1.*
- `doh` (optional): Descriptors of [RFC 8484](https://datatracker.ietf.org/doc/html/rfc8484) DNS-over-HTTPS endpoints
  - `doh[].url`: absolute URL of the endpoint. Outbound requests (in plaintext) will be sent to this URL. For `https` URLs, a `tls-client` plugin should be present in the outbound chain specified by `doh[].next`. Cautious should be taken when using domain names in the host part, which may cause infinite loops if the outbound chain references the plugin itself. A [`redirect`](./redirect.md) plugin is not needed as the Stream destination is set to the authority part of the URL.
  - `doh[].next`: Descriptor of the Stream Outbound to establish new outbound Streams.

## Details

The `host-resolver` plugin is powered by [`trust_dns_resolver`](https://docs.rs/trust-dns-resolver/latest/trust_dns_resolver/).

The DNS-over-HTTPS functionality is powered by [`hyper`](https://docs.rs/hyper/latest/hyper/) HTTP Client. Both `h2` and `http/1.1` are supported. `h2` will be used when selected by the lower layer [`tls-client`](./tls-client.md) plugin through [ALPN](https://en.wikipedia.org/wiki/Application-Layer_Protocol_Negotiation).

## Revision History

- 2023-04-29: Removed `map_back` related content.
- 2023-06-05: Added `doh`.
