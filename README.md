# Twitter for Node.js

A Promise-oriented asynchronous client library for the Twitter [REST](https://dev.twitter.com/rest/public) and [Streaming](https://dev.twitter.com/streaming/overview) API's.  
This is developed based on [node-twitter](https://github.com/desmondmorris/node-twitter), however, is __NOT COMPATIBLE__ with the original library.

Both of the REST methods are redesigned to be Thenable and can be chained with methods `then`.

```javascript
var Twitter = require('twitter');

var client = new Twitter({
  consumer_key: '',
  consumer_secret: '',
  access_token_key: '',
  access_token_secret: ''
});

var params = {screen_name: 'nodejs'};
client.get('statuses/user_timeline', params).then(function(tweets, response) {
  console.log(tweets);
}, function(err, response) {
  console.log(err);
}).then(function() {
  console.log('Done.');
});
```

## Quick Start

You will need valid Twitter developer credentials in the form of a set of consumer and access tokens/keys.  You can get these [here](https://apps.twitter.com/).  Do not forgot to adjust your permissions - most POST request require write permissions.

```javascript
var Twitter = require('twitter');

var client = new Twitter({
  consumer_key: '',
  consumer_secret: '',
  access_token_key: '',
  access_token_secret: ''
});
```

Add your credentials accordingly.  I would use environment variables to keep your private info safe.  So something like:

```javascript
var client = new Twitter({
  consumer_key: process.env.TWITTER_CONSUMER_KEY,
  consumer_secret: process.env.TWITTER_CONSUMER_SECRET,
  access_token_key: process.env.TWITTER_ACCESS_TOKEN_KEY,
  access_token_secret: process.env.TWITTER_ACCESS_TOKEN_SECRET,
});
```

You now have the ability to make GET and POST requests against the API via the convenience methods.

```javascript
client.get(path, params).then(resolveCallback, rejectCallback);
client.post(path, params).then(resolveCallback, rejectCallback);
client.stream(path, params, callback);
```

## REST API

You simply need to pass the endpoint and parameters to one of convenience methods.  Take a look at the [documentation site](https://dev.twitter.com/rest/public) to reference available endpoints.

Example, lets get a [list of favorites](https://dev.twitter.com/rest/reference/get/favorites/list):

```javascript
client.get('favorites/list').then(function(tweets, response) {
  console.log(tweets);  // The favorites.
  console.log(response);  // Raw response object.
}, function(error, response) {
  throw error;
});
```

How about an example that passes parameters?  Let's  [tweet something](https://dev.twitter.com/rest/reference/post/statuses/update):

```javascript
client.post('statuses/update', {status: 'I Love Twitter'}).then(function(tweet, response) {
  console.log(tweet);  // Tweet body.
  console.log(response);  // Raw response object.
}, function(error, response) {
  throw error;
});
```

## Streaming API

Using the `stream` convenience method, you to open and manipulate data via a stream piped directly from one of the streaming API's. Let's see who is talking about javascript:

```javascript
client.stream('statuses/filter', {track: 'javascript'}, function(stream) {
  stream.on('data', function(tweet) {
    console.log(tweet.text);
  });

  stream.on('error', function(error) {
    throw error;
  });
});
```

## Examples

* [Tweet](https://github.com/desmondmorris/node-twitter/tree/master/examples#tweet)
* [Search](https://github.com/desmondmorris/node-twitter/tree/master/examples#search)
* [Streams](https://github.com/desmondmorris/node-twitter/tree/master/examples#streams)
* [Proxy](https://github.com/desmondmorris/node-twitter/tree/master/examples#proxy)
* [Media](https://github.com/desmondmorris/node-twitter/tree/master/examples#media)

## Contributors

### node-twitter
Originally authored by  [@technoweenie](http://github.com/technoweenie)
 and maintained by [@jdub](http://github.com/jdub)

Currently maintained by [@desmondmorris](http://github.com/desmondmorris)

[And we cannot forget the community](https://github.com/desmondmorris/node-twitter/graphs/contributors)


## LICENSE

node-twitter-promise: Copyright (c) 2015 Chitoku
node-twitter: Copyright (c) 2014 Desmond Morris

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
