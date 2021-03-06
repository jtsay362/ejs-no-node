# EJS-No-Node

Embedded JavaScript templates for non-Node environments

This is based off TJ's ejs library, but with Node.js stuff removed to make it
run on Rhino as a module. It is used in the
[Solve for All Javascript execution environment](https://solveforall.com/docs/developer/javascript_execution).

The execution environment allows developers to write plugins to the
[Solve for All](https://solveforall.com/) answer (search) engine in Javascript.

## Installation

* In a browser environment,

    <script src="scripts/ejs-no-node.js"></script>

  ("ejs_no_node" should now be available in the global scope (window))

* In a CommonJS environment (Node.js, Rhino),

    var ejs = require('ejs-no-node');

* In a Solve for All plugin,

    const ejs = require('ejs');

## Features

  * Unbuffered code for conditionals etc `<% code %>`
  * Escapes html by default with `<%= code %>`
  * Unescaped buffering with `<%- code %>`
  * Supports tag customization
  * Filter support for designer-friendly templates
  * Client-side support
  * Newline slurping with `<% code -%>` or `<% -%>` or `<%= code -%>` or `<%- code -%>`

## Added Features

  * Ability to output function strings that are not directly executable (e.g. ES6 code that needs to be transpiled)

## Removed Features compared to tj/ejs

* Complies with the [Express](http://expressjs.com) view system
* Static caching of intermediate JavaScript
* Includes

## Example

    <% if (user) { %>
	    <h2><%= user.name %></h2>
    <% } %>

## Try out a live example now

<a href="https://runnable.com/ejs" target="_blank"><img src="https://runnable.com/external/styles/assets/runnablebtn.png" style="width:67px;height:25px;"></a>

## Usage

    ejs.compile(str, options);
    // => Function

    ejs.render(str, options);
    // => str

    ejs.compileToFunctionString(str, options);
    // => str, e.g. 'function anonymous(locals, filters, escape, rethrow) { ... }'

## Options

  - `scope`           Function execution context
  - `debug`           Output generated function body
  - `compileDebug`    When `false` no debug instrumentation is compiled
  - `client`          Returns standalone compiled function
  - `open`            Open tag, defaulting to "<%"
  - `close`           Closing tag, defaulting to "%>"
  - `functionName`    For compileToFunctionString(), the name of the function to generate, defaulting to "anonymous"
  - *                 All others are template-local variables

## Custom delimiters

Custom delimiters can also be applied globally:

    var ejs = require('ejs');
    ejs.open = '{{';
    ejs.close = '}}';

Which would make the following a valid template:

    <h1>{{= title }}</h1>

## Filters

EJS conditionally supports the concept of "filters". A "filter chain"
is a designer friendly api for manipulating data, without writing JavaScript.

Filters can be applied by supplying the _:_ modifier, so for example if we wish to take the array `[{ name: 'tj' }, { name: 'mape' },  { name: 'guillermo' }]` and output a list of names we can do this simply with filters:

Template:

    <p><%=: users | map:'name' | join %></p>

Output:

    <p>Tj, Mape, Guillermo</p>

Render call:

    ejs.render(str, {
        users: [
          { name: 'tj' },
          { name: 'mape' },
          { name: 'guillermo' }
        ]
    });

Or perhaps capitalize the first user's name for display:

    <p><%=: users | first | capitalize %></p>

## Filter list

Currently these filters are available:

  - first
  - last
  - capitalize
  - downcase
  - upcase
  - sort
  - sort_by:'prop'
  - size
  - length
  - plus:n
  - minus:n
  - times:n
  - divided_by:n
  - join:'val'
  - truncate:n
  - truncate_words:n
  - replace:pattern,substitution
  - prepend:val
  - append:val
  - map:'prop'
  - reverse
  - get:'prop'

## Adding filters

 To add a filter simply add a method to the `.filters` object:

```js
ejs.filters.last = function(obj) {
  return obj[obj.length - 1];
};
```

## License

(The MIT License)

Copyright (c) 2009-2010 TJ Holowaychuk &lt;tj@vision-media.ca&gt;

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
