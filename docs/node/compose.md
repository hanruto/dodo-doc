# 洋葱模型

> 洋葱模型是说中间价会按照约定的注册顺序依次执行，执行完毕后会从后往前依次执行await next()之后的逻辑，就像拨洋葱一样。

具体实现的核心是 compose 函数。这个函数的核心还是递归，把下一个函数作为next传给当前函数，然后在当前函数中就可以执行下一个函数，下一个里边也可以执行下下一个，以此类推。  

核心实现逻辑如下：

``` js
const middleware = [];
const ctx = {};

function use(callback) {
  middleware.push(callback);
}

use(async (ctx, next) => {
  console.log(1);
  await next()
  console.log(1);
})

use(async (ctx, next) => {
  console.log(2);
  await next()
  console.log(2);
})

use(async (ctx, next) => {
  console.log(3);
  await next()
  console.log(3);
})

function compose(middleware) {
  return async function (context, next) {
    async function dispatch(i) {
      if (i > middleware.length) {
        throw new Error('invalid middleware index');
      }

      if (i < middleware.length) {
        const fn = middleware[i];

        await fn(context, () => {
          i + 1 < middleware.length && dispatch(i + 1);
        });
      }

      if (i === middleware.length) {
        await next()
      }
    }

    return await dispatch(0);
  }
}

function start() {
  compose(middleware)(ctx)
}

start();
```
