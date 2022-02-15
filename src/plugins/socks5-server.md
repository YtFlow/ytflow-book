# socks5-server

SOCKS5 server.

## Access Points

- `tcp`: Stream Handler.

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
  "tcp_next": "forward.tcp",
  "udp_next": "forward.udp",
}
```

- `user` (optional): Specify the user name of the credential. Omitting this field disables authentication.
- `pass` (optional): Specify the password of the credential. Omitting this field disables authentication.
- `tcp_next`: Descriptor of the Stream Handler to use.
- `udp_next`: Descriptor of the Datagram Session Handler to use.
