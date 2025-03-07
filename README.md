
# oembed-parser

Extract eEmbed content from given URL.

[![NPM](https://badge.fury.io/js/oembed-parser.svg)](https://badge.fury.io/js/oembed-parser)
[![CI test](https://github.com/ndaidong/oembed-parser/workflows/ci-test/badge.svg)](https://github.com/ndaidong/oembed-parser/actions)
[![Coverage Status](https://coveralls.io/repos/github/ndaidong/oembed-parser/badge.svg?branch=main)](https://coveralls.io/github/ndaidong/oembed-parser?branch=main)
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=ndaidong_oembed-parser&metric=alert_status)](https://sonarcloud.io/dashboard?id=ndaidong_oembed-parser)
[![JavaScript Style Guide](https://img.shields.io/badge/code_style-standard-brightgreen.svg)](https://standardjs.com)


## Demo

- [Give it a try!](https://ndaidong.github.io/oembed-parser-demo)
- [Example FaaS](https://us-central1-technews-251304.cloudfunctions.net/oembed-parser?url=https://www.youtube.com/watch?v=8jPQjjsBbIc)


## Installation

```bash
$ npm install oembed-parser

# or pnpm
$ pnpm install oembed-parser

# or yarn
$ yarn add oembed-parser
```


## Usage

```js
const { extract } = require('oembed-parser')

// es6 module syntax
import { extract } from 'oembed-parser'

// test
const url = 'https://www.youtube.com/watch?v=8jPQjjsBbIc'

extract(url).then((oembed) => {
  console.log(oembed)
}).catch((err) => {
  console.trace(err)
})
```

## APIs

#### .extract(String url [, Object params])

Load and extract oembed data.

Example:

```js
const { extract } = require('oembed-parser')

const getOembed = async (url) => {
  try {
    const oembed = await extract(url)
    return oembed
  } catch (err) {
    console.trace(err)
    return null
  }
}

const data = getOembed('your url')
console.log(data)
```

Optional argument `params` can be useful when you want to specify some additional customizations.

Here are several popular params:

- `maxwidth`: max width of embed size
- `maxheight`: max height of embed size
- `theme`: e.g, `dark` or `light`
- `lang`: e.g, 'en', 'fr', 'cn', 'vi', etc

Note that some params are supported by these providers but not by the others.
Please see the provider's oEmbed API docs carefully for exact information.

#### .hasProvider(String URL)

Check if a URL matches with any provider in the list.

Examples:

```js
const { hasProvider } = require('oembed-parser')

hasProvider('https://www.youtube.com/watch?v=ciS8aCrX-9s') // return true
hasProvider('https://trello.com/b/BO3bg7yn/notes') // return false
```

#### .findProvider(String URL)

Get the provider which is relevant to given URL.

For example:

```js
const { findProvider } = require('oembed-parser')

findProvider('https://www.facebook.com/video.php?v=999999999')
```

Result looks like below:

```json
{
  fetchEndpoint: 'https://graph.facebook.com/v10.0/oembed_video',
  providerName: 'Facebook',
  providerUrl: 'https://www.facebook.com/'
}
```

#### .setProviderList(Array providers)

Apply a list of providers to use, overriding the [default](https://raw.githubusercontent.com/ndaidong/oembed-parser/master/src/utils/providers.json).

This can be useful for whitelisting only certain providers, or for adding
custom providers.

Default list of resource providers is synchronized from [oembed.com](http://oembed.com/providers.json).

For example:

```js
const { setProviderList } = require('oembed-parser')

const providers = [
  {
    provider_name: 'Alpha',
    provider_url: 'https://alpha.com',
    endpoints: [
      // endpoint definition here
    ]
  },
  {
    provider_name: 'Beta',
    provider_url: 'https://beta.com',
    endpoints: [
      // endpoint definition here
    ]
  }
]

setProviderList(providers)
```

#### .setRequestOptions(Object requestOptions)
Define options to call oembed HTTP request.

`oembed-parser` is using [axios](https://github.com/axios/axios) to send HTTP requests. Please refer [axios' request config](https://axios-http.com/docs/req_config) for more info.

Default option:

```js
{
  headers: {
    'user-agent': 'Mozilla/5.0 (X11; Linux i686; rv:94.0) Gecko/20100101 Firefox/94.0',
    accept: 'application/json; charset=utf-8'
  },
  responseType: 'json',
  responseEncoding: 'utf8',
  timeout: 6e4,
  maxRedirects: 3
}
```


## Facebook and Instagram

Since October 24 2020, Facebook have deprecated their legacy urls and applied a new Facebook oEmbed endpoints.

Technically, now we have to use Facebook Graph API, with the access token from a valid and live Facebook app. `oembed-parser` will try to get these values from environment variables, so please define them, for example:

```bash
export FACEBOOK_APP_ID=your_app_id
export FACEBOOK_CLIENT_TOKEN=your_client_token
```

References:

- [oEmbed Read](https://developers.facebook.com/docs/features-reference/oembed-read)
- [Facebook oEmbed](https://developers.facebook.com/docs/plugins/oembed)
- [Facebook Graph API](https://developers.facebook.com/docs/graph-api/overview)

## Test

```bash
git clone https://github.com/ndaidong/oembed-parser.git
cd oembed-parser
npm install
npm test

# quick evaluation
npm run eval {URL_TO_PARSE_OEMBED}
```

## License
The MIT License (MIT)

---
