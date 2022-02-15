# shadowsocks-client

Shadowsocks client.

## Access Points

- `tcp`: Stream Outbound.

*The UDP protocol is not implemented as of YtFlowCore 0.1.*

## Parameters

```json
{
  "method": "aes-128-gcm",
  "password": {
    "__byte_repr": "utf8",
    "data": "my_ss_password"
  },
  "tcp_next": "proxy-redir.tcp",
  "udp_next": "null.udp"
}
```

- `method`: The encryption method, or cipher. Supported methods:
    - `none`/`plain`
    - `rc4`
    - `rc4-md5`
    - `aes-128-cfb`
    - `aes-192-cfb`
    - `aes-256-cfb`
    - `aes-128-ctr`
    - `aes-192-ctr`
    - `aes-256-ctr`
    - `camellia-128-cfb`
    - `camellia-192-cfb`
    - `camellia-256-cfb`
    - `aes-128-gcm`
    - `aes-256-gcm`
    - `chacha20-ietf`
    - `chacha20-ietf-poly1305`
    - `xchacha20-ietf-poly1305`
- `password`: Specify the password to generate encryption keys.
- `tcp_next`: Descriptor of the Stream Outbound to establish new outbound Streams.
- `udp_next`: Descriptor of the Datagram Session Outbound to establish new outbound Datagram Sessions.
