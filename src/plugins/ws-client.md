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

The WebSocket implementation is capable of initiating WebSocket requests over HTTP/2 when [`tls-client`](./tls-client.md) is used as the lower layer transport. In case the HTTP/2 connection handshake fails for any reason during the first connection attempt, HTTP/1.1 will be used for all subsequent requests.

Since the WebSocket protocol does not support half-closed connections, issues might occur for those user applications that rely on this feature.

## Revision History

- 2023-05-11: Added `ws-client`.
- 2024-10-16: Document WebSockets over HTTP/2
