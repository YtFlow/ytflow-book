# ws-client

WebSocket client.

## Access Points

- `tcp`: Stream Outbound. Returns a Stream on top of WebSocket.

## Parameters

```json
{
  "host": "dl.microsoft.com",
  "path": "/",
  "headers": {},
  "next": "proxy-redir.tcp"
}
```

- `host`: Specify the value of `Host` HTTP header.
- `path`: Specify the path of the HTTP request.
- `headers`: A key-value map of additional HTTP headers to be sent in the request.
- `next`: Descriptor of the Stream Outbound.

## Details

Currently the WebSocket handshakes are based on HTTP/1.1 connections. Do not include `h2` in `alpn` when [`tls-client`](./tls-client.md) is used as the lower layer transport.

Since the WebSocket protocol does not support half-closed connections, issues might occur for those user applications that rely on this feature.

## Revision History

- 2023-05-11: Added `ws-client`.
