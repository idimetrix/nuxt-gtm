# nuxt-gtm

[![npm version][npm-version-src]][npm-version-href]
[![npm downloads][npm-downloads-src]][npm-downloads-href]
[![Checks][checks-src]][checks-href]
[![Codecov][codecov-src]][codecov-href]
[![License][license-src]][license-href]

> Google Tag Manager Module for Nuxt.js

## Setup

1. Add `nuxt-gtm` dependency to your project

```bash
yarn add nuxt-gtm # or npm install nuxt-gtm
```

2. Add `nuxt-gtm` to the `modules` section of `nuxt.config.js`

```js
export default {
  modules: [
    'nuxt-gtm',
  ],
  gtm: {
    id: 'GTM-XXXXXXX'
  }
}
```
### Runtime Config

You can use [runtime config](https://nuxtjs.org/guide/runtime-config) if need to use dynamic environment variables in production. Otherwise, the options will be hardcoded during the build and won't be read from `nuxt.config` anymore.

```js
export default {
  modules: [
    'nuxt-gtm'
  ],

  gtm: {
    id: 'GTM-XXXXXXX', // Used as fallback if no runtime config is provided
  },

  publicRuntimeConfig: {
    gtm: {
      id: process.env.GOOGLE_TAG_MANAGER_ID
    }
  },
}
```

## Options

Defaults:

```js
export default {
  gtm: {
    enabled: undefined, /* see below */
    debug: false,

    id: undefined,
    layer: 'dataLayer',
    variables: {},

    pageTracking: false,
    pageViewEventName: 'nuxtRoute',

    autoInit: true,
    respectDoNotTrack: true,

    scriptId: 'gtm-script',
    scriptDefer: false,
    scriptAsync: true,
    scriptURL: 'https://www.googletagmanager.com/gtm.js',
    crossOrigin: false,

    noscript: true,
    noscriptId: 'gtm-noscript',
    noscriptURL: 'https://www.googletagmanager.com/ns.html'
  }
}
```

### `enabled`

GTM module uses a debug-only version of `$gtm` during development (`nuxt dev`).

You can explicitly enable or disable it using `enabled` option:

```js
export default {
  gtm: {
    // Always send real GTM events (also when using `nuxt dev`)
    enabled: true
  }
}
```

### `debug`

Whether `$gtm` API calls like `init` and `push` are logged to the console.

### Manual GTM Initialization

There are several use cases that you may need more control over initialization:

- Block Google Tag Manager before user directly allows (GDPR realisation or other)
- Dynamic ID based on request path or domain
- Initialize with multi containers
- Enable GTM on page level

`nuxt.config.js`:

```js
export default {
 modules: [
  'nuxt-gtm'
 ],
 plugins: [
  '~/plugins/gtm'
 ]
}
```

`plugins/gtm.js`:

```js
export default function({ $gtm, route }) {
  $gtm.init('GTM-XXXXXXX')
}
```

- **Note:** All events will be still buffered in data layer but won't send until `init()` method getting called.
- **Note:** Please consult with [Google Tag Manager Docs](https://developers.google.com/tag-manager/devguide#multiple-containers) for more information caveats using multiple containers.

### Router Integration

You can optionally set `pageTracking` option to `true` to track page views.

**Note:** This is disabled by default to prevent double events when using alongside with Google Analytics so take care before enabling this option.

The default event name for page views is `nuxtRoute`, you can change it by setting the `pageViewEventName` option.

## Usage

### Pushing events

You can push events into the configured layer:

```js
this.$gtm.push({ event: 'myEvent', ...someAttributes })
```

## Development

1. Clone this repository
2. Install dependencies using `yarn install` or `npm install`
3. Start development server using `yarn dev` or `GTM_ID=<your gtm id> yarn dev` if you want to provide custom GTM_ID.

## License

[MIT License](./LICENSE)

Copyright (c) Nuxt.js Community

<!-- Badges -->
[npm-version-src]: https://img.shields.io/npm/v/nuxt-gtm/latest.svg?style=flat-square
[npm-version-href]: https://npmjs.com/package/nuxt-gtm

[npm-downloads-src]: https://img.shields.io/npm/dt/nuxt-gtm.svg?style=flat-square
[npm-downloads-href]: https://npmjs.com/package/nuxt-gtm

[checks-src]: https://img.shields.io/github/workflow/status/nuxt-community/gtm-module/test/master?style=flat-square
[checks-href]: https://github.com/nuxt-community/gtm-module/actions

[codecov-src]: https://img.shields.io/codecov/c/github/nuxt-community/gtm-module.svg?style=flat-square
[codecov-href]: https://codecov.io/gh/nuxt-community/gtm-module

[license-src]: https://img.shields.io/npm/l/nuxt-gtm.svg?style=flat-square
[license-href]: https://npmjs.com/package/nuxt-gtm
