# Preface

The YtFlowCore Book is prepared for those who want to customize their [YtFlowCore](https://github.com/YtFlow/YtFlowCore) (or its UI front ends like [YtFlowApp](https://github.com/YtFlow/YtFlowApp)) Profiles.

## Prerequisites

To edit YtFlow profiles, you may need a dedicated editor. YtFlowApp has a built-in configuration editor. The `ytflow-edit` [TUI](https://en.wikipedia.org/wiki/Text-based_user_interfaces) tool from YtFlowCore also works.

Although we are trying to minimize necessary prior knowledge, understanding of network fundamentals can greatly improve your experience while exploring YtFlow. You are not expected to be a network expert (while some of you already are) to get started, as long as you can tell the difference between TCP and UDP.

Since we present plugin parameters with JSON, you will need to get familiar with the JSON format. Can you spot a mistake in this JSON document?

```rust
# use serde_json::{from_str, Value};
# let s = r#"
{
    "host": "localhost",
    "port": 1080,
    "password": "password",
    "tcp_next": "socket",
}
# "#;
# println!("{:?}", from_str::<Value>(&s[1..]));
```

## About This Book

The YtFlowCore Book is open source and available at [GitHub](https://github.com/YtFlow/ytflow-book). Feel free to contribute by submitting a new issue to request clarification, or making pull requests to fix typos.

---

<a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-nd/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/">Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International License</a>.
