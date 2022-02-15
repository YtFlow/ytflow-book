# http-obfs-server

simple-obfs HTTP server.

## Access Points

- `tcp`: Stream Handler. Consumes an HTTP request from a Stream and reply with an HTTP response.

## Parameters

```json
{
  "next": "forward.tcp"
}
```

- `next`: Descriptor of the Stream Handler to consume the Stream handshaked with HTTP headers.

## Details

This plugin is compatible with [simple-obfs](https://github.com/shadowsocks/simple-obfs). It extracts the value of `Sec-Websocket-Key` from HTTP request headers and replies with HTTP response headers. It does not perform real WebSocket handshaking.
