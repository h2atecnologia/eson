
# ejson

  Extended JSON for node.

## Parser

  Currently only the parser portion is implemented, useful for configuration files.
  For example a typical configuration file might look something like the following:

```json
{
  "views": "/www/example.com/views",
  "view engine": "jade",
  "poll interval": 5000,
  "canvas size": { "width": 800, "height": 600 }
}
```

 With Extended JSON you can define plugin functions, or use ones
 bundled with ejson to transform the input, allowing for more
 declarative configurations as shown here:

```json
{
  "views": "{root}/views",
  "view engine": "jade",
  "poll interval": "5 seconds",
  "canvas size": "800x600"
}
```

### Writing plugins

 Writing a plugin is simple, it's a function which takes the signature `(key, val, parser)`. Let's write one that transforms every value to "foo":

```js
function foo(key, val, parser) {
  return 'foo';
}
```

 Then use the plugin like so:

```js
var ejson = require('ejson');

var conf = ejson()
  .use(foo)
  .read('path/to/config.json');
```

 Now suppose `path/to/config.json` contained `{ "foo": "bar", "bar": "baz" }`,
 the `foo()` plugin would yield `{ "foo": "foo", "bar": "foo" }`. So you get the picture,
 with this we can accept arbitrary strings such as "5 seconds" and transform
 it to the more useful `5000` milliseconds representation.

## License 

(The MIT License)

Copyright (c) 2012 TJ Holowaychuk &lt;tj@vision-media.ca&gt;

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.