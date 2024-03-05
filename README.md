# quest-adapter
A proxy based HTTP requests utility

## Installation
`npm install --save quest-adapter`

## Examples
**quest-adapter** is a flexible HTTP lib, which utilises adapters of the form

```ts
interface Options {
  uri: string,
  method: string,
  body?: any,
}

type quest-adapterAdapter = (options:Options) => Promise<any>;
```

Included within is a simple adapter based on the **request** module.

### Usage with `request`
```js
const request = require('quest-adapter/request');
const quest-adapter = require('quest-adapter')(request, 'https://jsonplaceholder.typicode.com', { json: true });

async function updateFirstComment(postId, patch) {
  const [comment] = await quest-adapter.posts[postId].comments.get();
  Object.assign(comment, patch);
  await quest-adapter.comments[comment.id].put(comment);
  return true;
}
```

### Frontend Usage
You can use use the special `quest-adapter/es5` module to import an uglifiable version of this lib. Here's an example using it and the builtin `fetch` function:

```js
function fetchAdapter(options) {
  options.method = options.method.toUpperCase();
  const body = new FormData();
  body.append('json', JSON.stringify(options.body));
  options.body = body;
  return fetch(options.uri, options).then(response => response.json());
}

const quest-adapter = require('quest-adapter/es5')(fetchAdapter, 'https://jsonplaceholder.typicode.com');

async function updateFirstComment(postId, patch) {
  const [comment] = await quest-adapter.posts[postId].comments.get();
  Object.assign(comment, patch);
  await quest-adapter.comments[comment.id].put(comment);
  return true;
}
```

#### A note about `hyphenate`
**quest-adapter** options all have a `hyphenate` key that is set to `true` by default. It ensures that calling props like `quest-adapter.userControls` will be translated to the url `/user-controls`. If your API does'nt follow this convention, you can set it to `false` globally or individually.
