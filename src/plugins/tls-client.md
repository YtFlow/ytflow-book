# tls-client

TLS client stream.

## Access Points

- `tcp`: Stream Outbound. Encrypt the Stream with TLS.

## Parameters

```json
{
  "alpn": ["h2", "http/1.1"],
  "skip_cert_check": false,
  "sni": "my.trojan.proxy.server.com",
  "next": "proxy-redir.tcp"
}
```

- `alpn` (optional): List of [ALPN] values. If omitted, the ALPN extension will be inferred from the upper layer plugins, or disabled if none of the upper layer plugins provide ALPN values. Do not include `h2` when [`ws-client`](./ws-client.md) is used as the upper layer transport, which only supports HTTP/1.1.
- `skip_cert_check` (optional): Whether to skip certificate verification, a.k.a. `allowInsecure`. Enabling this option invites [MITM attacks](https://en.wikipedia.org/wiki/Man-in-the-middle_attack), thus you are strongly discouraged from doing so.
- `sni` (optional): Overwrite the server name in TLS requests. If omitted, the destination hostname of Streams will be used.
- `next`: Descriptor of the Stream Outbound to send TLS encrypted data.

## Details

The following plugins are [ALPN]-aware:

- [`ws-client`](./ws-client.md): `http/1.1` only
- [`host-resolver`](./host-resolver.md): `h2` and `http/1.1` for DNS-over-HTTPS endpoints

## Revision History

- 2023-05-10: Added `alpn`.
- 2023-06-05: Specify auto-inferred `alpn`.

[ALPN]: https://en.wikipedia.org/wiki/Application-Layer_Protocol_Negotiation
