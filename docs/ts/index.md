## ts是怎么工作的？

tsconfig.json 用来定义ts的编译方式和一些参数。

把ts文件转化成js，可以用tsc命令，也可以用webpack或者babel来转化，前端一般会用webpack来打包代码，webpack通过ts-loader来分析ts代码然后进行打包，而服务端会使用tsc来直接转化ts代码为js代码。

声明文件用来辅助声明类型

## .d.ts 声明文件

#### 声明文件的作用

为什么需要声明文件呢，在使用一些没有 ts 类型的库的时候，比如jQuery，开发的时候 ts 可没办法知道我们在 index.html 引入了一个 jquery.min.js 所以全局会有 $ 这么个变量，所以需要开发者手动的告诉ts。

比如创建一个 env.ts文件，然后写上下面的一行声明，那么其他地方就可以正常的使用 $ 这个方法了。

``` ts
declare function $(selector: string): any;
```

所以声明文件的作用就是对一些没有ts类型的变量，方法，类等等，进行显示的声明，它可以声明的东西非常多种多样。

#### 通过引入第三方包来得到类型声明

但是众所周知 jquery 的方法非常复杂，比如 `$('#a').html()` 或者 `$.ajax().then()`，这导致写声明的时候也会变得非常艰难。其实这个问题也可以通过第三方依赖解决。比如安装 @types/jquery 就可以引入完整的jquery的ts类型，
当然 @types/jquery 做的也只不过是帮忙写好了所有的声明。

#### 声明文件的写法

通常这种为了区分声明文件和常规的代码逻辑，会把这种文件写做env.d.ts，d即declare。  

1. 声明全局变量
在声明文件里可以声明任意变量。最常见的就是声明一些全局变量，或者给window上挂载一些方法或者变量。

``` ts
declare var a: number;

declare function foo(): any;

declare interface Window { hello: string };
```

除此之外，还经常可以声明一些引入的文件类型。


2. 声明文件引用

``` ts
/// <reference path="./declare/window.d.ts" />
```

3. 声明模块
加入声明一个 hello 可以 export 一个 world 变量，那么我们就可以认为有一个 'hello' 的模块可以导出 world 这个变量。
``` ts
declare module 'hello' {
  export const world: string
}
```

``` ts
import { world } from 'hello';
```

这个有什么用呢，我们通常 import 一些文件的时候，比如图片，比如一个 vue 后缀名的文件，ts 并不知道他们导出了些什么，所以这个声明就可以告诉 ts，这些引用做了些什么。
 
比如这个就可以告诉 ts, 我们引入一个 .png 结尾的后缀名的文件的时候，其实是引入了一个 string;

``` ts
declare module "*.png" {
  const link: string;
  export default link;
}
```

除此之外可以也可以通过声明模块来给一些没有ts类型的第三方模块提供声明。

## tsconfig.json

typescript 可以把ts文件转化成js文件，需要用到一个命令行工具tsc，转化的时候有很多参数，比如转化成es3，es5，还是es6。  


``` bash
tsc --target es6
```

当参数比较多的时候，写在命令行里就不够方便了，所以会有tsconfig.json这个文件，tsc编译的时候会读取这个文件，当然命令行里的参数的优先级会更高。


#### 文件选择和排除相关参数

tsconfig.json意味着该文件所在的目录会是将要执行的编译的根目录。可以通过 files, include, exclude 来指定那些文件会被编译，哪些不会被编译。

include 和 files 指定的文件是入口文件，即使其他文件不在匹配范围内，只要被引用了就会顺便被编译了。

#### 编译行为

- compileOptions.target 编译成什么版本的语言比如es3 es5 es6

- compilerOptions.sourceMap 是否生成 sourceMap 文件

- compilerOptions.outDir 编译出来的文件放到哪里

- compilerOptions.baseUrl 路径映射的基础路径，和paths配合使用

- compilerOptions.allowJs 和 compilerOptions.checkJs 是否允许编译和检查 js

- compilerOptions.incremental 是否开启增量编译

- compilerOptions.tsBuildInfoFile 增量编译的缓存路径

- compilerOptions.declaration 生成声明文件

- compilerOptions.declarationDir 声明文件的路径

- compilerOptions.emitDeclarationOnly 只生成声明文件，开启之后outDir就失效了

#### 严格模式

- strictNullChecks 

  开启了这个之后null, undefined这些就会被考虑到，否则下边的代码，就不会考虑color是undefined的情况。
  ``` ts
  const getColorLabel = (color?: Colors) => {
    return {
      [Colors.GREEN]: 'green',
      [Colors.YELLOW]: 'yellow',
    }[color];
  }
  ```


- noImplicitAny 是否开启不允许隐含的any
  ``` ts
  let a: any = 1;
  ```
  这种声明是显示的

  ``` ts
  const log = (data) => {
    console.log(data);
  }
  ```
  这个里的data就是一个隐式声明的any类型

- compilerOptions.paths 路径映射，注意这里的通配符的写法，不是路径的写法。比如按照下面的方式指定之后，就可以通过 @utils/index 来引用 ./src/utils/index 里的文件了
  ``` json
  "paths": {
    "@utils/*": [
      "./src/utils/*"
    ]
  }
  ```

  这个可以直接引用 logger 这个module
  ``` json
  "paths": {
      "logger": [
        "./src/libs/logger"
      ]
    }
  ```

- strictFunctionTypes 函数类型的参数也会严格检查

- strictBindCallApply bind、call 和 apply 语法的时候，是否进行参数检查

- strictPropertyInitialization 类的属性值必须被初始化

- noImplicitThis 不允许隐式的 this

- alwaysStrict 默认开启的，就是严格模式

#### 其他参数

- compileOnSave 是否开启保存文件自动编译

- extends 继承

- references 指定引用

