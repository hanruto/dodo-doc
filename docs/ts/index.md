## ts是怎么工作的？

tsconfig.json 用来定义ts的编译方式和一些参数。

把ts文件转化成js，可以用tsc命令，也可以用webpack或者babel来转化，前端一般会用webpack来打包代码，webpack通过ts-loader来分析ts代码然后进行打包，而服务端会使用tsc来直接转化ts代码为js代码。

.d.ts 用来辅助声明类型


## .d.ts 声明文件

#### 声明文件的作用

为什么需要声明文件呢，在使用一些没有 ts 类型的库的时候，比如jQuery，开发的时候 ts 可没办法知道我们在 index.html 引入了一个 jquery.min.js 所以全局会有 $ 这么个变量，所以需要开发者手动的告诉ts。

比如创建一个 env.ts文件，然后写上下面的一行声明，那么其他地方就可以正常的使用 $ 这个方法了。

``` ts
declare function $(selector: string): any;
```

所以声明文件的作用就是对一些没有ts类型的变量，方法，类等等，进行显示的声明，它可以声明的东西非常多种多样。

#### 通过引入第三方包来得到类型声明

但是众所周知 jquery 的方法非常复杂，比如 $('#a').html() 或者 $.ajax().then()，这导致写声明的时候也会变得非常艰难。其实这个问题也可以通过第三方依赖解决。比如安装 @types/jquery 就可以引入完整的jquery的ts类型，
当然 @types/jquery 做的也只不过是帮忙写好了所有的声明。

#### 声明文件的写法

通常这种为了区分声明文件和常规的代码逻辑，会把这种文件写做env.d.ts，d即declare。

## tsc


## ts-loader


## tsconfig.json


###
