## z语言
z语言[仓库](https://github.com/z-dev-group/zlang)目的是开发一个开箱即用（尤其是web项目），简单容易上手的语言，支持面向对象等等的语言

### 一、安装
目前在开发阶段，请使用源码安装的方式，依赖go，请提前安装好，后续会慢慢实现自举以及支持下载安装包。

1、克隆仓库

```
git clone git@github.com:z-dev-group/zlang.git
zlang/src/go/
make
```

2、设置Z_ROOT
```
cd ../../
export Z_ROOT=$(pwd)
```

3、执行z代码
```
./dist/z examples/hello.z
```

成功输出：
```
hello world
```

### 二、语法说明

#### 变量类型
z语言使用隐形变量申明方式，根据值反推变量类型，目前支持的类型比较简单，integer，float，string，boolean，object，array，null，error，function等
```
let age = 12
let height = 160.1
let name = "seven"
let address = {
  "province": "guangdong",
  "city": "guangzhou"
}
let fruits = ["apple", "banana"]
let is_passed = true
let do = fn() {

}
```

### 基本语法
* 语句不需要使用;结束
* 内置var_dump()输出变量信息

1、申明
[源码](./source/let.z)
使用let关键字申明变量，无需指明变量类型

```
let hello = "world"
var_dump(hello)
```
输出： 
```
world
```

2、表达式
[源码](./source/expression.z)
支持+，-，*，/，++，>, <, ==等常用表达式
```
let i = 0
i++
var_dump(i)
let compare = i == 1
var_dump(compare)
```
输出：
```
1
true
```

3、条件判断
[源码](./source/if.z)
使用if，else判断
```
let age = 36
if (age > 35) {
  var_dump("too old")
} else {
  var_dump("so young")
}
```
输出： 
```
too old
```

4、循环
支持while和for两种形式，支持break结束跳出循环
while
[源码](./source/while.z)
```
let i = 0
let total = 0
while (i < 10) {
  i++
  total = total + i
}
var_dump(total)
var_dump(i)

i = 0
total = 0
while (i < 10) {
  i++
  total = total + i

  if (i > 5) {
    break
  }
}
var_dump(total)
var_dump(i)
```
输出：
```
55
10
21
6
```
for 
[源码](./source/for.z)
```
let total = 0
for (let i = 0; i <= 10; i++) {
  total = total + i
}
var_dump(total)

total = 0
for (let j = 0; j <= 10; j++) {
  total = total + j
  if (j > 5) {
    break
  }
}
var_dump(total)
```
输出：
```
55
21
```
5、文件引入
加入有一个 import.z，需要在另外一个文件use_import.z使用import.z的内容，可以使用import导入
import.z
```
let name = "seven"
```
use_import.z
```
import "./import.z"
var_dump(name)
```
执行use_import.z
输出：
```
seven
```
6、包隔离
使用package关键字申明包，可以里面定义函数，类，变量
申明package
```
package string

let concat = fn(str_one, str_two) {
  return str_one + " " + str_two
}
```

使用package中函数，需要在函数前携带上包名
```
import "./package.z"

var_dump(string.concat("hello", "world"))
```
输出：
```
hello world
```

7、对象
z语言支持常规的面向对象编程，支持关键字class，new，interface，extends，implement，->等等操作
使用class申明类
```
class User {
  fn sayHello(name) {
    return "hello " + name
  }
}
```
使用new实例化对象
```
let user = new User()
```
使用->调用对象的方法
```
var_dump(user->sayHello("seven"))
```
输出：
```
hello seven
```
[源码](./source/class.z)

#### 错误处理
z语言使用变量携带错误信息的方式来实现异常处理，就一个普通的变量，我们可以给这个变量带上错误信息（抛出异常），可以检查一个变量是否带有错误信息（捕获异常），可以获取到异常的错误信息，避免go语言这种需要返回多个字段，以及java这种需要try catch 包裹的麻烦。
[源码](./source//error.z)
```
let name = "seven"

if (is_with_error(name)) {
  var_dump("find error")
} else {
  var_dump("not find error")
}

with_error(name, "this is user error")

if (is_with_error(name)) {
  var_dump("find error after with_error")
  var_dump(get_error_message(name))
}
```
预计输出：
```
not find error
find error after with_error
this is user error
```

#### 功能扩展
使用系统调用扩展z语言功能，包括创建处理，进程控制等等
[源码](./source/syscall.z)
```
fn getpid() {
  let result = syscall(20)
  result["result1"]
} 

fn getppid() {
  let result = syscall(39)
  result["result1"]
}

var_dump(getpid())
var_dump(getppid())
```
输出不确定，根据当时运行情况决定
### 三、todo

* 支持函数defer模块， defer {} 代码块的方式执行函数收尾代码
* 面向对象支持 this-> 指向自身
* 支持系统调用，用于封装更多内置模块