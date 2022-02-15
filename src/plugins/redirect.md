# redirect

Change the destination of connections or datagrams.

## Access Points

- `tcp`: Stream Outbound. Change the destination of Streams.
- `udp`: Datagram Session Outbound. Change the destination of Datagram Sessions and all datagrams flowing through.

## Parameters

```json
{
  "dest": {
    "host": "my.proxy.server.com.",
    "port": 8388
  },
  "tcp_next": "phy-socket",
  "udp_next": "phy-socket"
}
```

- `dest.host`: Hostname or IP address of the destination.
- `dest.port`: Destination port.
- `tcp_next`: Descriptor of the Stream Outbound to establish new outbound Streams.
- `udp_next`: Descriptor of the Datagram Session Outbound to establish new outbound Datagram Sessions.
