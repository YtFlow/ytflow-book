# http-proxy-client

HTTP Proxy client. Use HTTP CONNECT to connect to the proxy server.

## Access Points

- `tcp`: Stream Outbound. Initiates an HTTP CONNECT request on the stream, and consumes the response.

## Parameters

```json
{
  "user": {
      "__byte_repr": "utf8",
      "data": ""
  },
  "pass": {
      "__byte_repr": "utf8",
      "data": ""
  },
  "tcp_next": "proxy-redir.tcp"
}
```

- `user`: Specify the user name of the credential. An empty string disables authentication.
- `pass`: Specify the password of the credential. An empty string disables authentication.
- `tcp_next`: Descriptor of the Stream Outbound to send HTTP proxy requests.
