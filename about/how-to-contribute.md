# You want to help? That's fantastic!

```
git clone https://github.com/everydayhero/how-we-build-things
cd how-we-build-things
npm install gitbook-cli -g
npm install
gitbook serve
```

Some useful links to read more

- [The gitbook documentation](https://www.gitbook.com/book/gitbookio/documentation/details) is useful when you're trying to figure out how to improve things.
- [Mermaid](http://knsv.github.io/mermaid/#mermaid) lets you add diagrams like this one..

{% mermaid %}
graph TD;
  A-->B;
  A-->C;
  B-->D;
  C-->D;
{% endmermaid %}

- Github-style codeblocks work, too.

```js
"use strict";

var React = require('react');
var App = require('./components/App');
var ReactDOM = require('react-dom');


ReactDOM.render(<App/>, document.getElementById('counter'));
```

