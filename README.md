# zipkin-axios

This is fork of [zipkin-instrumentation-axios](https://github.com/uschmann/zipkin-instrumentation-axios), which is not maintained any more....

I need this lib to be upgraded to fix build warnigs when used at browser.

You need to inject you axios instance into **wrapAxios(axios, options)**.

## Installation

```bash
npm install --save zipkin-axios
```

## Usage

```javascript
// Require dependencies
const axios = require('axios');
const wrapAxios = require('zipkin-axios');
const { Tracer, ExplicitContext, BatchRecorder } = require('zipkin');

// Setup zipkin components
const ctxImpl = new ExplicitContext();
const recorder = new BatchRecorder({
  logger: new HttpLogger({
    endpoint: `http://localhost:9411/api/v1/spans`
  })
});
const tracer = new Tracer({ ctxImpl, recorder });

// Wrapp an instance of axios
const zipkinAxios = wrapAxios(axios, { tracer, serviceName: 'myService'});

// Fetch data with HTTP-GET
zipkinAxios.get('http://another-service/foo')
.then(result => res.send(result.data))
.catch(e => console.log(e));

// Post data
zipkinAxios.post('http://another-service/bar', { bar: 42 })
.then(result => res.send(result.data))
.catch(e => console.log(e));
```
