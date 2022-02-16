# ip-stack

Handle TCP or UDP connections from a TUN.

## Access Points

No access point is provided. This plugin should be used as entry plugin.

## Parameters

```json
{
  "tun": "uwp-vpn-tun.tun",
  "tcp_next": "rev-resolver.tcp",
  "udp_next": "dns-dispatcher.udp"
}
```

- `tun`: Descriptor of the TUN device to send and receive packets.
- `tcp_next`: Descriptor of the Stream Handler.
- `udp_next`: Descriptor of the Datagram Session Handler.

## Details

`ip-stack` is similar to `tun2socks`. It encapsulates Streams and Datagram Sessions from raw IP packets flow through the underlying TUN interface.

> Currently the endpoint IPv4 address is a hardcoded value `192.168.3.1`. IPv6 packets are discarded.
