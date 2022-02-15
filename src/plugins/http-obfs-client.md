# http-obfs-client

simple-obfs HTTP client.

## Access Points

- `tcp`: Stream Outbound. Returns a Stream handshaked with HTTP headers.

## Parameters

```json
{
  "host": "dl.microsoft.com",
  "path": "/",
  "next": "proxy-redir.tcp"
}
```

- `host`: Specify the value of `Host` HTTP header.
- `path`: Specify the path of the HTTP request.
- `next`: Descriptor of the Stream Outbound to perform HTTP obfuscation.

## Details

This plugin is compatible with [simple-obfs](https://github.com/shadowsocks/simple-obfs), and does not perform HTTP response checking or real WebSocket handshaking. It simply appends an HTTP request header before the Stream, and discards the response header.

Example of a generated HTTP response:

```http
GET / HTTP/1.1
Host: dl.microsoft.com
User-Agent: curl/7.5.0
Upgrade: websocket
Connection: Upgrade
Sec-Websocket-Key: b7ZPFMiCJm6AZJ3dPx9OcQ==
Content-Length: 233

[233 bytes payload]
```
