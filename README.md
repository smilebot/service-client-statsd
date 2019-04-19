# @vrbo/service-client-statsd
![travis-ci-badge](https://travis-ci.org/homeaway/service-client-statsd.svg?branch=master)

A [Service Client](https://github.com/homeaway/service-client) plugin for reporting operational metrics to a [StatsD](https://github.com/statsd/statsd) daemon.

## Contents
* [Usage](#usage)
* [Configuration Options](#configuration-options)
* [Development](#development)
* [Further Reading](#further-reading)

## Usage
```javascript
const ServiceClient = require('@vrbo/service-client');
const SCStatsD = require('@vrbo/service-client-statsd');

ServiceClient.use(SCStatsD);

const client = ServiceClient.create('myservice')

(async function() {
    await client.request({ method: 'GET', path: '/v1/test/endpoint', operation: 'foobar' })
    /**
     * Outputs metrics to StatsD daemon:
     * localhost.serviceclient.myservice.foobar.statusCode200 method=increment
     * localhost.serviceclient.myservice.foobar.statusCode2xx method=increment
     * localhost.serviceclient.myservice.foobar.total method=timing value=1
     */
})()
```

See [plugin documentation](https://github.com/homeaway/service-client#plugins) for more usage information.

## Configuration Options
- **hostname** - The hostname used to initialize the internal instance of [Lynx](https://github.com/dscape/lynx). Defaults to `'localhost'`.
- **port** - The port used to initilize the internal instance of [Lynx](https://github.com/dscape/lynx). Defaults to `8125`.
- **lynxOptions** - Additional configuration options used to initilize the internal instance of [Lynx](https://github.com/dscape/lynx).
- **transmit** - A boolean indicating whether or not to actually send these metrics to the [Lynx](https://github.com/dscape/lynx) instance. Defaults to `true`.
- **prefix** - An optional value to prepend to the beginning of the metrics key reported to StatsD.

Example:
```javascript
const client = ServiceClient.create('myservice', {
    plugins: {
        statsd: {
            // options here
        }
    }
})
```

## Development
This package uses [`debug`](https://github.com/visionmedia/debug) for logging debug statements.

Use the `serviceclient:plugin:statsd` namespace to log related information:
```bash
DEBUG=serviceclient:plugin:statsd npm test
```

## Further Reading
* [License](LICENSE)
* [Code of conduct](CODE_OF_CONDUCT.md)
* [Contributing](CONTRIBUTING.md)
* [Changelog](CHANGELOG.md)
