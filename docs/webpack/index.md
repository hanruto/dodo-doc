# tree-shaking的原理
在 webpack 中设置 mode production 打包代码的时候就会启动 tree-shaking。   
tree-shaking 是对 js 的语法进行静态分析之后移除掉未被引入的代码。这个理念最早在 rollup 中被提出，在 webpack4 中被正式引入，在模块打包的时候，如果引入一个新的 package, package 的 sideEffects 属性被设置为 false（默认false）, 那么就会对引入的代码进行 tree-shaking，tree-shaking 只能在打包好项目之后进行，编译好之后会擦除掉没用代码，它不仅可以擦除没用的模块，还能擦除没用的声明。

