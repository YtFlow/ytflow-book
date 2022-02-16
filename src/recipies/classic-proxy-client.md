# Classic Proxy Client

## Objectives

1. Build a Shadowsocks Client with a SOCKS5 inbound, akin to the classic Shadowsocks client.
3. Focus on TCP only.

## Plugins

```

 ┌──────────┐     ┌───────────────┐     ┌──────────────┐     ┌───────────┐
 │          │     │               │     │              │     │           │
 │ listener │ tcp │ socks5-server │ tcp │ main-forward │ tcp │ ss-client │
 │          ├────►│               ├────►│              ├────►│           │
 └──────────┘     └───────────────┘     └──────────────┘     └────┬──────┘
                                                                  │
                                        ┌──────────────┐          │
                                        │              │          │
                                        │ sys-resolver │          │ tcp
                                        │              │          │
                                        └───▲──────────┘          │
                                            │ resolver            │
                            ┌─────┐       ┌─┴──────────┐   ┌──────▼──────┐
                            │     │       │            │   │             │
                            │ phy │ netif │ phy-socket │   │ proxy-redir │
                            │     │◄──────┤            │◄──┤             │
                            └─────┘       └────────────┘   └─────────────┘

```

- `listener` ([`socket-listener`](../plugins/socket-listener.md))
```json
{
  "tcp_listen": ["127.0.0.1:1080"],
  "udp_listen": [],
  "tcp_next": "socks5-server.tcp",
  "udp_next": "reject.udp"
}
```
- `socks5-server` ([`socks5-server`](../plugins/socks5-server.md))
```json
{
  "tcp_next": "main-forward.tcp",
  "udp_next": "reject.udp"
}
```
- `main-forward` ([`forward`](../plugins/forward.md))
```json
{
  "tcp_next": "ss-client.tcp",
  "udp_next": "null.udp"
}
```
- `ss-client` ([`shadowsocks-client`](../plugins/shadowsocks-client.md))
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
- `proxy-redir` ([`redirect`](../plugins/redirect.md))
```json
{
  "dest": {
    "host": "my.proxy.server.com.",
    "port": 8388
  },
  "tcp_next": "resolve-dest.tcp",
  "udp_next": "null.udp"
}
```
- `phy-socket` ([`socket`](../plugins/socket.md))
```json
{
  "netif": "phy.netif",
  "resolver": "sys-resolver.resolver"
}
```
- `sys-resolver` ([`system-resolver`](../plugins/system-resolver.md))
```json
null
```
- `phy` ([`netif`](../plugins/netif.md))
```json
{
  "family_preference": "NoPreference",
  "type": "Auto"
}
```
- `reject` ([`reject`](../plugins/reject.md))
```json
null
```
- `null` ([`null`](../plugins/null.md))
```json
null
```

## Explanation

1. `listener` binds to a local host endpoint and listens for incoming connections.
2. For each incoming connection, `socks5-server` performs a SOCKS5 handshake and obtains a destination address from the SOCKS5 request, say `google.com:443`.
3. `main-forward` initiates a new connection with `ss-client`.
4. Since `ss-client` has encoded the destination address into the Shadowsocks protocol, `proxy-redir` will redirect the connection to the proxy address (`my.proxy.server.com.:8388`). Otherwise, encrypted Shadowsocks payload will be sent directly to `google.com:443`, which is not expected.
5. `phy-socket` resolves the IP address of `my.proxy.server.com.` using a [`system-resolver`](../plugins/system-resolver.md).
6. The Shadowsocks server at `my.proxy.server.com.:8388` receives the request and starts relay between `google.com:443` and your PC.
