# simple-dispatcher

Match the source/dest address against a list of simple rules, and use the corresponding handler or fallback handler if there is no match.

## Access Points

- `tcp`: Stream Outbound.
- `udp`: Datagram Session Outbound. Dispatching applies to Datagram Sessions only. Datagrams flowing through are not inspected.

## Parameters

```json
{
  "fallback_tcp": "main-forward.tcp",
  "fallback_udp": "main-forward.udp",
  "rules": [
    {
      "is_udp": true,
      "src": {
        "ip_ranges": [[0, [0, 0, 0, 0]]],
        "port_ranges": [{ "start": 0, "end": 65535 }]
      },
      "dst": {
        "ip_ranges": [[32, [11, 16, 1, 1]]],
        "port_ranges": [{ "start": 53, "end": 53 }]
      },
      "next": "fakeip-dns-server.udp"
    }
  ]
}
```

- `fallback_tcp`: Descriptor of the Stream Outbound to establish new outbound Streams when none of the rules matches.
- `fallback_udp`: Descriptor of the Datagram Session Outbound to establish new outbound Datagram Sessions when none of the rules matches.
- `rules`: List of rules.
    - `rules[].is_udp`: Specify whether the rule applies to Datagram Sessions or Streams.
    - `rules[].src`: Specify the source address and port.
        - `rules[].src.ip_ranges`: List of IP ranges.
            - `rules[].src.ip_ranges[][0]`: Prefix length.
            - `rules[].src.ip_ranges[][1]`: IP address, in array format.
        - `rules[].src.port_ranges`: List of port ranges.
            - `rules[].src.port_ranges[].start`: Start port, inclusive.
            - `rules[].src.port_ranges[].end`: End port, inclusive.
    - `rules[].dst`: Specify the destination address and port.
    - `rules[].next`: Descriptor of the handler to use when the rule matches.

## Note

`simple-dispatcher` matches all the rules sequentially, therefore not suitable for large number of rules.
