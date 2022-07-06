---
title: Initialization
editor: markdown
sidebar_position: 3
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

Once you have [installed](02_installation.md) the library and [created](04_configuration.md) your configuration
you will be able to initialize the server.

<Tabs groupdId="lang">
<TabItem value="commonjs" label="CommonJS">

```js
const Saffron = require("@poiw/saffron");

const saffron = new Saffron();

(async () => {
    // Initialize saffron
    await saffron.initialize({ /* configuration */})
    
    // Start sheduler to parse websites.
    await saffron.start();
})();
```

</TabItem>
<TabItem value="esmodules" label="ES modules">

```js
import Saffron from "@poiw/saffron";

const saffron = new Saffron();

(async () => {
    // Initialize saffron
    await saffron.initialize({ /* configuration */})

    // Start sheduler to parse websites.
    await saffron.start();
})();
```

</TabItem>
<TabItem value="typescript" label="TypeScript">

```ts
import Saffron from "@poiw/saffron";

const saffron = new Saffron();

(async () => {
    // Initialize saffron
    await saffron.initialize({ /* configuration */})

    // Start sheduler to parse websites.
    await saffron.start();
})();
```

</TabItem>
</Tabs>
