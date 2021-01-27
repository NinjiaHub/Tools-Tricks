# 动态导入 

webpack@4.x 支持动态导入模块，这是代码分割的一个实现方式，即在使用的地方导入模块加载资源。其实现方式如下：

```js
// src/index.js
async function getComponent() {
  const element = document.createElement('div');
  const { default: _ } = await import('lodash');

  element.innerHTML = _.join(['Hello', 'webpack'], ' ');

  return element;
}


setTimeout(() => {
  // getComponent()

  getComponent().then((component) => {
    document.body.appendChild(component);
  });
}, 2000)
```

上面的代码在运行 2s 之后执行 getComponent 方法，方法中动态加载了 lodash，这样就避免了在页面刚开始加载的时候就加载 lodash，即实现了按需加载，也避免了加载不必要的资源。在真实的场景中，我们可以用来在用户点击后加载某些模块，不点击就不加载对应的资源。

注意上面的 default 部分，官方文档解释：

> The reason we need default is that since webpack 4, when importing a CommonJS module, the import will no longer resolve to the value of module.exports, it will instead create an artificial namespace object for the CommonJS module. For more information on the reason behind this, read [webpack 4: import() and CommonJs](https://medium.com/webpack/webpack-4-import-and-commonjs-d619d626b655).

使用 webpack 支持的 dynamic import 时，导入的是一个对象，其中的 default 值为加载模块的默认导出。

## optimization

webpack 提供了打包优化，可以使打出来的包更符合使用场景，例如如果两个包都引用了同一个第三方包，如果不做优化的情况下，这个第三方包会打包进两个调用的包中，加载的时候相当于该第三方包下载了两次；如果使用了优化，则可以单独打一个包，在页面加载时单独加载一次。

```js
module.exports = {
  mode: 'development',
  target: 'web',
  entry: {
    main: './src/index.ts',
  },
  optimization: {
    runtimeChunk: 'single',
    splitChunks: {
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          chunks: 'all',
        },
      },
    },
  },
}
```

注意这里有一个陷阱或者说冲突，这里会把所有 node_modules 目录下的第三方包打成一个 `vendor.js` 文件；而 `src/index.js` 中通过动态引入此时可以不再使用；因为第三方包 lodash 被打入了 `vendor.js` 文件中加载了一次，而等到 2s 后执行 `getComponent` 方法时，会再次加载一次webpack打出的动态引入的包，这个包中也包含了 lodash。所以在配置 optimization 时需要注意 dynamic import 的实现部分是否也引入了第三方包，避免优化失效。

## Typescript 中的 dynamic import

```ts
// src/index.ts
async function getComponent() {
  const element = document.createElement('div');
  const _ = await import('lodash');

  element.innerHTML = _.join(['Hello', 'webpack'], ' ');

  return element;
}
```

在动态引入部分，如果项目中使用的是 typescript，则需要注意，ts 自己内部实现了 dynamic import，而该实现与 webpack 不太一样的地方就是，引入的模块直接按照 esModule 的方式使用，没有 default 这一层包装。

## 声明

本文部分内容参考自网络，如有版权问题请联系作者。如内容有错误或者不恰当的地方，敬请指正。

侵删。

作者邮箱：web.taox@gmail.com。
