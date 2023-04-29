# Access Point and Descriptor

An access point is like a "listening port" of a plugin. A plugin can expose access points for other plugins to connect to, and connect to access points of other plugins as well. A descriptor specifies the plugin name and the access point that a plugin connects to.

## Types of Access Point

Access points are typed, which means you cannot arbitrarily connect a plugin to an access point. Currently, there are 7 types of access points:

- Stream Handler
- Datagram Session Handler
- Stream Outbound
- Datagram Session Outbound
- Resolver
- TUN
- Netif

We will discuss these types and explain why they are categorized as such in the following sections.

### Streams, Stream Handler and Stream Outbound

In YtFlowCore, a TCP connection is represented by a Stream. A plugin that work with TCP streams can implement a new Stream on top of an underlying Stream to receive and send protocol headers (e.g. [SOCKS5](https://en.wikipedia.org/wiki/SOCKS)), or to encrypt and decrypt data (e.g. [Shadowsocks](https://en.wikipedia.org/wiki/Shadowsocks)).

A Stream Handler *handles* streams. When a Stream is produced by an upstream plugin, it is passed to the Stream Handler. The Stream Handler may do some I/O, wrap a new Stream around it, pass it to a downstream Stream Handler, or even terminate the Stream. Stream Handlers are often used on the inbound side.

A Stream Outbound works the other way around. It *initiates* streams. An upstream plugin asks the Stream Outbound for a new Stream upon request. Then, the Stream Outbound creates a new Stream, or reuses a Stream from a downstream Stream Outbound, and returns it to the upstream plugin. Stream Outbounds are often used on the outbound side.

To exchange data from two Streams, a [`forward`] plugin is used. It exposes a Stream Handler and requires a Stream Outbound to work. When a Stream is received from the Stream Handler access point, the plugin requests a new Stream from the Stream Outbound, and then forwards data between the two streams. Note that [`forward`] only forwards data, and does nothing about outbound selection, routing or DNS resolution.

### Datagram Sessions, Datagram Session Handler and Datagram Session Outbound

As you may have guessed, Datagram Session is the UDP counterpart of Stream. A Datagram Session represents a UDP receipient, to which a plugin can send or receive datagrams.

Datagram Session Handler and Datagram Session Outbound work the same way as Stream Handler and Stream Outbound, but they deal with Datagram Sessions. [`forward`] is also used to forward datagrams between two Datagram Sessions.

### Resolver

A Resolver can be used to fulfill requests related to domain names, such as resolving domain names to/from IP addresses, or possibly other type of requests like [TXT](https://en.wikipedia.org/wiki/TXT_record) or [SRV](https://en.wikipedia.org/wiki/SRV_record). The requests and responses use internal representations, thus a Resolver does not necessarily work with raw DNS payloads directly. Rather, a [`dns-server`](./plugins/dns-server.md) should be used to serve DNS messages and [`host-resolver`](./plugins/host-resolver.md) should be used to send requests to a real DNS server.

### TUN

A TUN is used to send and receive Layer 3 Raw IP packets. Currently, only [`ip-stack`](./plugins/ip-stack.md) requires a TUN to extract Streams and Datagram Sessions.

## Descriptor

Descriptors are used to locate access points. Usually, it is the combination of a plugin name and a predefined short string, separated by a dot. For example, `trojan-outbound.tcp` may refer to the Stream Outbound access point of a [`trojan-client`](./plugins/trojan-client.md) plugin called `trojan-outbound`.

Moreover, a descriptor may refer to an access point with different types. For example, a [`socket`] plugin called `phy-socket` has a descriptor named `phy-socket`, which is both a Stream Outbound and Datagram Session Outbound.

[`forward`]: ./plugins/forward.md
[`socket`]: ./plugins/socket.md

## Revision History

- 2023-04-29: Removed Netif.
