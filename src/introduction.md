# Introduction

[YtFlowCore] is a modern proxy framework that allows heavy customization. You can design your own network flow by chaining plugins and tuning parameters. Expert users will find it interesting to play around with custom profiles, while novice users may choose to benefit from preset configurations that our UI front end, such as [YtFlowApp], has set up for you.

To achieve maximum flexibility, the plugins are designed with [KISS principle](https://en.wikipedia.org/wiki/KISS_principle) in mind. Sometimes, you will find it cumbersome to deploy so many plugins, only to achieve something appears as a simple button in other apps. However, such explicitness avoids confusion when you debug and customize your network. For example, the FakeIP infrastructure can be decomposed into:

- A FakeIP DNS resolver, providing fake IP addresses and corresponding domain names,
- A DNS server, acting as 'FakeDNS', and
- A destination resolver to translate FakeIP back into domain names from incoming connections.

These plugins are fully configurable, and work intuitively in your profile. Let's say you want to implement split routing based on countries/regions of destination addresses[^geoip]. In order to lookup geolocations from the database, you will need a real-IP DNS resolver to get IP addresses associated with domain names. You may ask, "will the real-IP DNS resolver mess up with the FakeIP one?" The answer is, no. In this profile, a destination address will only be overwritten by the destination resolver, while DNS resolvers do no change the destination address. You will learn more about plugins in [Chapter 4](./plugins.md).

[YtFlowCore]: https://github.com/YtFlow/YtFlowCore
[YtFlowApp]: https://github.com/YtFlow/YtFlowApp

[^geoip]: There should be a plugin called `geoip-dispatcher` to achieve that, but it has not been implemented in [YtFlowCore].
