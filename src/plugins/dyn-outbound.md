# dyn-outbound

Select an outbound proxy from the database at runtime.

> This plugin requires a database file to work. If a database file is not provided, it will fail to load.

## Access Points

- `tcp`: Stream Outbound.
- `udp`: Datagram Session Outbound.

## Parameters

```json
{
  "tcp_next": "ss-client.tcp",
  "udp_next": "null.udp"
}
```

- `tcp_next`: Descriptor of the Stream Outbound for proxies to establish new outbound Streams.
- `udp_next`: Descriptor of the Datagram Session Outbound for proxies to establish new outbound Datagram Sessions.

## Details

Proxies in the database are managed by the UI application. During runtime, a user can select a proxy from the UI via RPC. The selection will be stored in the database so that the plugin will automatically load the selected proxy on next startup.

If a selected proxy cannot be loaded during plugin initialization, all incoming connections will be rejected until another proxy is selected and loaded successfully.

## Revision History

- 2023-04-29: Added `dyn-outbound`.
