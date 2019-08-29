# WARNING
# WARNING
# WARNING
# WARNING
This package is still in development and not well tested.


# controller-api-wrapper
Generate api wrapper by existing controller

# example
Please check example/koa for more details

```
/// app.ts
import Koa from 'koa';
import Router from 'koa-router';
import bodyParser from 'koa-bodyparser';
import { flatten } from 'controller-api-wrapper';

import routers from './routers';

const app = new Koa();
const router = new Router();


let arr = flatten(routers);
arr.forEach(v => {
  console.log(`Adding to router, name: ${v.name}, url: ${v.url}, method: ${v.method}`)
  if (v.method === "get") {
    router.get(v.name, v.url, (ctx, next) => {
      const arg = { ...ctx.query, ...ctx.params };
      console.log(arg);
      v.handler(arg);
    });
  } else {
    router.get(v.name, v.url, (ctx, next) => {
      const arg = { ...ctx.request.body, ...ctx.params };
      console.log(arg);
      v.handler(arg);
    });
  }
})

app.use(bodyParser())
app.use(router.routes());

module.exports = app;

if (!module.parent) app.listen(3000);
```

Generate wrapper.generated.js
```
// gen.ts
import { generateJs } from 'controller-api-wrapper';
import routers from './routers';

generateJs(routers);
```
```
$ npm run build && node lib/gen
```


# License
MIT License
