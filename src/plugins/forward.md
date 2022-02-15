# forward

Establish a new connection for each incoming connection, and forward data between them.

## Access Points

- `tcp`: Stream Handler. Forward data between incoming Streams and newly established outbound Streams.
- `udp`: UDP Stream Handler. Forward datagrams between incoming Datagram Sessions and newly established outbound Datagram Sessions.

## Parameters

```json
{
  "request_timeout": 200,
  "tcp_next": "ss-client.tcp",
  "udp_next": "ss-client.udp"
}
```

- `request_timeout` (optional, defaults to `100`): Specify how long (in milliseconds) to wait for initial data from the inbound Stream. `0` means do not wait for initial data, and establish outbound connections immediately. This option does not apply to Datagram Sessions. See [Details](#details) for more information.
- `tcp_next`: Descriptor of the Stream Outbound to establish new outbound Streams.
- `udp_next`: Descriptor of the Datagram Session Outbound to establish new outbound Datagram Sessions.

## Details

To enable potential 0-RTT capability for the entire outbound link, `forward` plugin tries reading some data from the inbound Stream before establishing an outbound Stream, and pass the data all the way to downstream Stream Outbounds. Thus, downstream plugins can take advantage of TLS 1.3 0-RTT or TCP Fast Open features. In most cases, this is OK because common Layer 7 protocols like HTTP or TLS send request data immediately after establishing a connection.
