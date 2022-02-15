# socket-listener

Bind a socket to a specified port and listen for connections or datagrams.

## Access Points

No access point is provided. This plugin should be used as entry plugin.

## Parameters

```json
{
  "tcp_listen": ["127.0.0.1:1080"],
  "udp_listen": ["127.0.0.1:1080"],
  "tcp_next": "geoip-dispatcher.tcp",
  "udp_next": "geoip-dispatcher.udp"
}
```

- `tcp_listen` (optional): List of socket addresses to listen on TCP connections.
- `udp_listen` (optional): List of socket addresses to receive UDP datagrams.
- `tcp_next`: Descriptor of the Stream Handler.
- `udp_next`: Descriptor of the Datagram Session Handler.
