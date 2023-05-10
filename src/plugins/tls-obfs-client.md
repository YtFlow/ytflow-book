# tls-obfs-client

simple-obfs TLS client.

## Access Points

- `tcp`: Stream Outbound. Returns a fake-TLS handshaked Stream.

## Parameters

```json
{
  "host": "dl.microsoft.com",
  "next": "proxy-redir.tcp"
}
```

- `host`: Specify the value of `Host` HTTP header.
- `next`: Descriptor of the Stream Outbound to perform HTTP obfuscation.

## Details

This plugin is compatible with [simple-obfs](https://github.com/shadowsocks/simple-obfs), and does not perform real TLS handshaking. It simply appends a fake ClientHello packet before the Stream, and discards the response header.

## Revision History

- 2023-05-11: Added `tls-obfs-client`.
