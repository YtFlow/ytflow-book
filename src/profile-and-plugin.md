# Profile and Plugin

Profile and Plugin are two core entities in YtFlow. When YtFlowCore launches, it retrieves the specified profile from the database, along with all plugins included in the profile. Then, the plugins are loaded and started to respond to incoming connections. Errors occur during parsing or loading plugins will be reported to the user. In this case, YtFlowCore may or may not reject running the profile, depending on the implementation.

## Profile

A profile consists of a collection of plugins that collectively defines runtime behavior of YtFlowCore. There is not too much interesting fields inside a profile itself. Instead, you may customize the network flow by modifying the plugins.

Remember that a YtFlowCore instance only runs one profile at a time. If you want to switch to a different profile, you will need to restart the YtFlowCore instance, or reconnect through the UI.

## Plugin

A plugin in a profile is an atomic instance of certain functions, such as stream encoding and decoding, destination rewrite, and dispatching to different plugins. They are designed to be as small as possible, so that you can make the most of them by chaining them in a creative way.

You may have already noticed that outbound client plugins, such as [`shadowsocks-client`](./plugins/shadowsocks-client.md) and [`socks5-client`](./plugins/socks5-client.md), care nothing about which address or port your proxy server is running on. They only encode  destination addresses into the streams, but do not change the destination. To redirect all streams to the proxy server, you will need a [`redirect`](./plugins/redirect.md) plugin.

> Note that plugins are a integrated part of YtFlowCore. Currently, there is no such mechanism to dynamically load a new type of plugin from a shared library.

### Parameters

To customize a plugin, you modify the parameters of it. For each plugin, the parameters are encoded in a [CBOR] object. You don't have to know what [CBOR] exactly is, because the editor will automatically convert [CBOR] to JSON and vice versa. Sometimes you will see such object in the editor:

```json
{
  "__byte_repr": "utf8",
  "data": "my_trojan_password"
}
```

Don't be scared. Most of the time, you can treat them as strings with content of the `data` field. You see this because the editor encountered a binary buffer field, which may possibly contain non-UTF-8 data. In this case, the actual data is displayed in [Base64](https://en.wikipedia.org/wiki/Base64) format.

```json
{
    "__byte_repr": "base64",
    "data": "3q2+7w=="
}
```

A plugin can have many access points. Plugins chain to each other by specifying names and access points of other plugins in its parameters.

### Entry Plugin

Some plugins like [`socket-listener`](./plugins/socket-listener.md) and [`ip-stack`](./plugins/stack.md) are made to work as entry plugins. They are supposed to be the entry points of the whole profile, and therefore do not provide any access point.

In the editor, you can set a plugin as entry plugin, or unset to deactivate it. There can be multiple entry plugins in a profile. Note that every plugin can be an entry plugin, but most plugins will have no effect at runtime.

[CBOR]: https://cbor.io/
