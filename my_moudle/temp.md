## Node 基础知识

### 模块组织代码

Node通过`模块`可以对代码进行组织和包装，使其更容易重用。
```
相比于PHP，PHP是通过定义命名空间来避免变量和函数冲突，但仍可能污染全局命名空间。比如第三方的函数命名与你本地有冲突，很难修改。
```

`模块`主要使用`require`引入，而导出则通过`exports`和`module.exports`。以下是一个基本使用的例子：

currency.js
```
const canadianDollar = 0.91;

const roundTwo = (amount) => {
    return Math.round(amount * 100) /100;
};

exports.canadianToUS = canadian => roundTwo(canadian * canadianDollar);
exports.USToCanadian = us => roundTwo(us / canadianDollar);
```

test_currency.js
```
const curreny = require('./currency');
console.log('50 Canadian dollars equals this amount of US dollars:');
console.log(curreny.canadianToUS(50));
console.log('30 US dollars equals this amount of Canadian dollars:');
console.log(curreny.USToCanadian(30));
```

#### 导入

1. `require`一般放在程序最初加载的地方（一是因为顶部引入更简洁，二是`require`是同步操作，会阻塞Node，比较适合放在程序最初加载的地方）

2. 引入时，找模块的顺序：核心模块->当前目录下的node_modules下模块->父目录模块->NODE_PATH指定的目录。

3. 同一程序引入模块会将对象缓存起来。不同文件引入相同模块，后者会从内存中去取。



#### 导出

`exports`是一个可添加属性的空对象，属性可以是函数或者变量。
而`module.exports`是向外提供单个函数、变量或对象。若两者同时使用`exports`会被忽略，原因是`exports.myFun`其实是`module.exports.myFun`的缩写，同时使用会打破两者的引用关系，导致`exports`被忽略

### 异步