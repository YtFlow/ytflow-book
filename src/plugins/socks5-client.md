# socks5-client

SOCKS5 client.

## Access Points

- `tcp`: Stream Outbound.

*The UDP protocol is not implemented as of YtFlowCore 0.1.*

## Parameters

```json
{
  "user": {
    "__byte_repr": "utf8",
    "data": "my_socks5_username"
  },
  "pass": {
    "__byte_repr": "utf8",
    "data": "my_socks5_password"
  },
  "tcp_next": "proxy-redir.tcp",
  "udp_next": "proxy-redir.udp",
}
```

- `user` (optional): Specify the user name of the credential. Omitting this field disables authentication.
- `pass` (optional): Specify the password of the credential. Omitting this field disables authentication.
- `tcp_next`: Descriptor of the Stream Outbound to send SOCKS5 requests.
- `udp_next`: Descriptor of the Datagram Session Outbound to send SOCKS5 UDP payloads.
