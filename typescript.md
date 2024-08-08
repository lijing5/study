# typescript



## 安装

运行命令：

````js
npm i typescript -g
````

安装完成后运行命令：

````js
tsc -v
````

如果出现版本号则代表安装成功



## 开发环境配置

tsc工具可以为我们初始化一个typescript的简单开发环境，只需运行：

````js
tsc --init
````

运行结束后，会生成一个`tsconfig.json`文件，这个文件就是你需要的开发配置文件

运行 tsc -w 可以生成一个.js文件



安装

```
运行 nodejs 环境执行ts
npm i @types/node --save-dev （node环境支持的依赖必装）
npm i ts-node --g
```

使用ts-node 文件名.ts可以运行ts



## 编译

TS是一个预编译的语言，所以我们每一次都需要先将TS编译完成才可以运行产出的JS文件，如何编译呢？

按照以下步骤尝试一下：

### 创建TS文件

在目录内创建一个`xxx.ts`文件，并书写一段代码：

````js
console.log('hello world')
````



### 执行编译

我们有两种方式执行编译：

#### 手动编译

运行命令：

````js
tsc ./01-ts基础.ts
````



#### 自动编译

**第一步**

打开`tsconfig.json`文件，找到`rootDir`，这个配置用于指定TS文件所在的目录。

我们将这个配置修改为`./ts`，意思是所有编译行为只对根目录下的`./ts`目录有效

同时在根目录创建一个`./ts`目录



**第二步**

继续查找`outDir`，将这个配置修改为`./js`目录，意思是所有编译后的文件产出都在`./js`目录下。

同时在根目录创建一个`./js`目录



**第三步**

在控制台运行命令：

````js
tsc -w
````

这个命令是以监听模式执行编译，此后我们只要修改了ts文件，则tsc会自动执行编译操作



## 类型约束

Typescript最大的特点就是类型约束，这让JS代码具备了静态语言的能力。

所谓静态语言其实就是指变量类型一旦确定就不允许轻易修改的语言特性，诸如java，c#等等都是静态语言



Typescript如何约束类型呢？这个问题我们需要拆成两方面来看

### 基本类型约束

常用的基本类型约束有：

- string
- number
- boolean
- null
- undefined

以上类型的约束非常简单：

````typescript
const 变量名:类型 = 变量值
````



### 引用类型约束

引用类型的约束我们常用的有三种情况：

- 数组
- 函数
- 对象

#### 数组

数组的约束语法有几种

形态约束法：

````js
const arr:number[] = [1,2,3,4,5]
````

泛型声明法：

````typescript
const arr1:Array<number> = [1,2,3,4,5]
````



#### 函数

函数的约束要分为三种情况：

- 返回值约束
- 参数约束
- 函数体结构约束

**返回值约束**

我们在函数小括号后面使用`:类型`来约束函数的返回值

> 如果函数没有返回值，注意此处说的是明确没有return。那么我们使用`:void`来约束

````typescript
function fn1():number[]{
    return [1,2,3,4,5]
}
````

**参数约束**

函数的参数约束是在函数的参数后面加上`:类型`

```typescript
//注意，参数不能多传，也不能少传 必须按照约定的类型来
function fn2(arg1: number, arg2: number):number {
    return arg1 + arg2
}

fn2(2,3)

//如果参数是一个对象，用interface去定义
interface User {
    name:string,
    age:number
}

function add(user:User):User{
    return User
}
```

**函数可选参数**

```typescript
//通过?表示该参数为可选参数
const fn = (name: string, age?:number): string => {
    return name + age
}
fn('张三')
```

**函数的默认值**

```typescript
const fn = (name: string = "我是默认值"): string => {
    return name
}
fn()
```

**函数体结构约束**

当我们使用函数表达式来声明一个函数的时候，如何确定它的类型结构呢？

把鼠标放到变量上就能看到了：

````typescript
const fn3:(arg1:string,arg2:string) => string = function(arg1:string,arg2:string){
    return arg1+arg2
}
````

**函数重载**

重载是方法名字相同，而参数不同，返回类型可以相同也可以不同。

如果参数类型不同，则参数类型应设置为 **any**。

参数数量不同你可以将不同的参数设置为可选。

```typescript
let user：number[]=[1,2,3]
function findNum():number[]  //如果什么都没有传查询全部
function findNum(id:number):number[]  //如果传入了id就是查询单个
function findNum(add:number[]):number[]  //如果传入的是一个number类型的数组那就做添加
function findNum(ids?:number | number[]):number[]{   //实现函数
    if(typeof ids=='number'){
        return user.filter(v=>v==ids)
    }else if(Array.isArray(isd)){
        user.push(...ids)
        return user
    }else{
        return user
    }
}
```



#### 对象

对象约束，我们还是先用鼠标来观察一下，你会发现得到的是一个与对象体特别相似的结构：

````typescript
const stuInfo:{
    name:string;
    age:number;
    isStu:boolean
} = {
    name:'张三',
    age:20,
    isStu:true
}
````

我们将对象、函数、数组这样的按照`结构+子成员`来进行约束的方案称为：结构子类型化

> 所谓结构子类型，代表着当前变量被赋予的值必须结构和子成员类型都满足要求才算是正确



### typescript自定义类型

typescript含有一些JavaScript不含有的 自定义类型，这些类型可以帮助我们更好的去实现TS项目的开发

#### void

不用过多接受，`void`的作用就是代表函数没有任何`return`行为

#### any

`any`是typescript中的顶级类型，所有类型都是any类型的子集，所以这代表着，如果你为一个变量约束为any，那么编译器会主动放弃对当前变量的类型检测



使用any的原则是：

- 你不关系这个数据的结构
- 你无法得知这个数据的结构

#### unknown

`unknown`也是typescript中的顶级类型，但`unknown`相比于`any`而言其实可以被看做是`any`的一种安全策略类型

unknown 只能赋值给自身 或者是any

unknown  没有办法读取任何属性  方法也不可以调用

```typescript
let stu:any={name:"小满",open:()=>123}
```

`unknown`与`any`的区别是：

- `any`可以赋值给任何一个变量，赋值执行过程中该变量会因为赋值目标是`any`的原因也被ts编译器放弃检测
- `unknown`不可以直接赋值给其他类型变量，如果赋值的话需要明确当前`unknown`即将成为的类型（请看后续断言小节）



## 断言

有了类型，那么如果在某个计算过程中我们非得要指定一个其他类型呢？（比如将`unknown`指定为一个明确类型才可以赋值）

我们可以使用断言语法，TS中断言的意思是强制要求某个变量变成某一个近似类型（一般为包含关系、交集关系）

断言语法有两种：

````typescript
//语法：值 as 类型 或　<类型>值 
varData1 as 类型

<类型>varData1	// 不建议在react中使用，因为会被误认为是——组件标签
````

需要注意的是，类型断言只能够「欺骗」TypeScript 编译器，无法避免运行时的错误，反而滥用类型断言可能会导致运行时错误：

```typescript
let someValue: any = 42; // 实际是一个number
let strLength: number = (someValue as string).length; // 类型断言告诉TypeScript这是一个string
```



## 高级联合类型

 联合类型（Union Types）表示取值可以为多种类型中的一种。 

当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们**只能访问此联合类型的所有类型里共有的属性或方法**：

```typescript
function getLength(something: string | number): number {
    return something.length;
}

// index.ts(2,22): error TS2339: Property 'length' does not exist on type 'string | number'.
//   Property 'length' does not exist on type 'number'.
//上例中，length 不是 string 和 number 的共有属性，所以会报错。
```

为了应对复杂数据场景TS提供了高级联合类型，其实就是给定变量一个类型范围，最终该变量会根据得到的具体值来确定自身的类型。 联合类型的变量在被赋值的时候，会根据类型推论的规则推断出一个类型： 

````typescript
const sth:(number[]|{name:string}) = [1,2,3]

sth.length  // ok: 3
sth.name // error:类型“number[]”上不存在属性“name”
//第二行的 sth.length  被推断成了 number[]，访问它的 length 属性不会报错。
//而第三行的 sth.name 被推断成了 {name:string}，访问它的 length 属性时就报错了。
````

上述代码中sth得到`[1,2,3]`以后，会自动定位成`number[]`类型，此后我们可以使用所有数组相关的方法，但不可以使用对象相关的操作



由联合类型其实可以得出另一个问题：**如果函数的参数也是联合类型，应该怎么办？**

比如，我有以下函数：

````typescript
function fn(arg:number[]|{name:string}){
    if(arg.name){   // 此处会报错，因为arg参与运算时并没有明确自身的类型，只有你实际传值的时候才会明确

    }
}
````

上述就会出现联合类型中`得到具体值才能定位类型`的问题，那么我们如何顺利处理呢？

两套方案：

- 断言
- 类型谓语



#### 断言处理

使用断言可以让当前联合类型在**当前执行行，被声明为明确类型**，但是后续使用都必须一致断言声明

````typescript
function fn(arg:number[]|{name:string}){
    if((arg as number[]).length){   // 此处会报错，因为arg参与运算时并没有明确自身的类型，只有你实际传值的时候才会明确
        return (arg as number[]).reduce((preVal:number,currentVal:number)=>{
            return preVal+currentVal
        },0)
    }

    if((arg as {name:string}).name){
        return (arg as {name:string}).name
    }
}
````

上述代码看起来真的非常非常难受



#### 类型谓语

何为类型谓语？其实就是**在当前执行作用域内，一开始就直接告知编译器这个变量的明确类型，此后该作用域直到结束时编译器都会一直沿用该变量的类型**

如何定义类型谓语呢？

其实就是一个**返回值类型为一句话的函数**

````typescript
/* 
        语法特点：
            - 一般函数名is打头
            - 参数就一个，参数的类型就是高级联合类型
            - 内部执行时将参数断言为某一个具体类型，并得出true的结果
            - 函数返回值修饰为：参数 is 具体类型
    */
function isNumberArr(arr: number[] | { name: string }): arr is number[] {
    return (arr as number[]).length !== undefined
}

function isObj(arr: number[] | { name: string }): arr is { name: string } {
    return (arr as { name: string }).name !== undefined
}

function fn2(arg: number[] | { name: string }) {
    if (isNumberArr(arg)) {
        return arg.length
    }

    if(isObj(arg)){
        return arg.name
    }
}
````

## 交叉类型

 多种类型的集合，联合对象将具有所联合类型的所有成员 

```typescript
interface People {
  age: number,
  height： number
}
interface Man{
  sex: string
}
const xiaoman = (man: People & Man) => {
  console.log(man.age)
  console.log(man.height)
  console.log(man.sex)
}
xiaoman({age: 18,height: 180,sex: 'male'});
```



## 泛型

泛型存在的目的，就是为了解决封装复用的问题，假设请求stu_list的接口，它既能传递`{currentPage,pageSize}`也能传递`{currentPage,pageSize,stu_name,stu_city,stu_birthday}`

````typescript
function getStuList(params:{
    currentPage:number;
    pageSize:number;
    stu_name:string;
    city:string;
    stu_birthday:Array<string>
}){

}

getStuList({currentPage:1,pageSize:5})	// error: 实参传递的类型结构不符合参数的类型约束
getStuList({currentPage:1,pageSize:5,stu_name:'string',city:'string',stu_birthday:['2022-05-21']})	// ok: 实参传递结构复合类型约束
````

上述代码无法实现复用，因为在最开始定义的时候我们就已经将形参的类型约束死了，此时我们需要引入泛型概念

那么什么是泛型？

**泛型，顾名思义就是宽泛的类型；需要我们将思维从传统JS只能定义数据形参的方向上转换到TS允许我们同时定义数据形参和类型形参！**

````typescript
 function fn<T>(arg:T){

 }

fn<string>('你好')
````

上述代码中我们将实参的类型也进行了抽象化，要求用户在传入数据实参的同时也必须传入一个类型实参



> 假如你希望你封装的过程能够被复用，那么泛型可以作为一个很好的思维方式



## 接口

### 基本概念

在 TypeScript 中，我们使用接口（Interfaces）来定义对象的类型。 

如果每一次声明变量、定义参数都需要显式的一次又一次书写相同的结构类型的话，对我们来说真是一个非常繁琐的过程，比如：

`````typescript
namespace Mod09 {
    function fn(params: {
        currentPage: number;
        pageSize: number;
        stu_name: string;
        city: string;
        stu_birthday: Array<string>
    }) {

    }


    const stuInfo: {
        currentPage: number;
        pageSize: number;
        stu_name: string;
        city: string;
        stu_birthday: Array<string>
    } = {
        currentPage: 1,
        pageSize: 5,
        stu_name: '张三',
        city: '北京',
        stu_birthday: ['2022-05-21']
    }

    const stuList: {
        currentPage: number;
        pageSize: number;
        stu_name: string;
        city: string;
        stu_birthday: Array<string>
    }[] = [
            {
                currentPage: 1,
                pageSize: 5,
                stu_name: '张三',
                city: '北京',
                stu_birthday: ['2022-05-21']
            }
        ]
}
`````

上述代码可以说是一次灾难，因为相同的数据结构约束我们写了三遍，以后或许还会接着写！

那么TS提出了接口概念来为我们解决这个问题， 我的理解是使用interface来定义一种约束，让数据的结构满足约束的格式 。

首先接口必须使用关键词`interface`来声明：

````typescript
interface 接口名 {
    key:类型;
    key:类型;
    ....
}
````

利用接口来解决上面的代码，会得到：

````typescript
namespace Mod09 {
    interface StuInfo {
        currentPage: number;
        pageSize: number;
        stu_name: string;
        city: string;
        stu_birthday: Array<string>
    }

    function fn(params: StuInfo) {

    }


    const stuInfo: StuInfo = {
        currentPage: 1,
        pageSize: 5,
        stu_name: '张三',
        city: '北京',
        stu_birthday: ['2022-05-21']
    }

    const stuList: StuInfo[] = [
            {
                currentPage: 1,
                pageSize: 5,
                stu_name: '张三',
                city: '北京',
                stu_birthday: ['2022-05-21']
            }
        ]
}
````

### 重名interface

```typescript
//重名interface  可以合并
interface A{name:string}
interface A{age:number}
var x:A={name:'xx',age:20}

//继承
interface A{
    name:string
}
 
interface B extends A{
    age:number
}
 
let obj:B = {
    age:18,
    name:"string"
}
```

### 任意属性

需要注意的是，一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集( 确定属性和可选属性的类型不能与索引签名的属性类型冲突 )：

```typescript
//在这个例子当中我们看到接口中并没有定义C但是并没有报错
//应为我们定义了[propName: string]: any;
//允许添加新的任意属性
interface Person {
    b?:string,   //可选属性
    a:string,    //确定属性
    [propName: string]: any;   //索引签名
}

const person:Person  = {
    a:"213",
    c:"123"
}
//在这个Person接口中，[key: string]: any;是一个索引签名。这表示Person对象可以有任意多个字符串属性名的属性，而对应的属性值的类型是any。因为索引签名的类型是any，所以确定属性name的类型string和可选属性age的类型number都是any的子集，所以这是合法的。
 
//举个不合法例子
interface Person {
    [key: string]: string; // 索引签名指定属性值必须为字符串类型
    name: string;          // 确定属性，类型为字符串，符合索引签名
    age?: number;          // 可选属性，类型为数字，与索引签名冲突
}
//在这个例子中，age属性的类型number不是索引签名中声明的string类型的子集，因此TypeScript编译器会报错，指出类型不兼容。

let tom: Person = {
    name: 'Tom',
    age: 25,
    gender: 'male'
};

// index.ts(3,5): error TS2411: Property 'age' of type 'number' is not assignable to string index type 'string'.
// index.ts(7,5): error TS2322: Type '{ [x: string]: string | number; name: string; age: number; gender: string; }' is not assignable to type 'Person'.
//   Index signatures are incompatible.
//     Type 'string | number' is not assignable to type 'string'.

//上例中，任意属性的值允许是 string，但是可选属性 age 的值却是 number，number 不是 string 的子属性，所以报错了。另外，在报错信息中可以看出，此时 { name: 'Tom', age: 25, gender: 'male' } 的类型被推断成了 { [x: string]: string | number; name: string; age: number; gender: string; }，这是联合类型和接口的结合。
```

 一个接口中只能定义一个任意属性。如果接口中有多个类型的属性，则可以在任意属性中使用联合类型： 

```typescript
interface Person {
    name: string;
    age?: number;
    [propName: string]: string | number;
}

let tom: Person = {
    name: 'Tom',
    age: 25,
    gender: 'male'
};
```

### 可选属性

我们都知道接口约束是非常严格的，你必须完全按照接口要求的结构来定义你的对象，但是有时候我们不见得非得需要某个属性，此时我们可以将该属性设置为可选的，语法特征：**在该属性名后加上`?`**

````typescript
interface TeacherParams {
    currentPage: number;
    pageSize: number;
    name?: string;
    teacher_birthday?: string
}

const teacherParams:TeacherParams = {
    currentPage: 1,
    pageSize: 5
}

const filterTeacher:TeacherParams = {
    currentPage: 1,
    pageSize: 5,
    name: '阎君'
}
````

### 只读属性

有时候我们希望对象中的一些字段只能在创建的时候被赋值，那么可以用 `readonly` 定义只读属性： 

```typescript
interface Person {
    readonly id: number;
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    id: 89757,
    name: 'Tom',
    gender: 'male'
};

tom.id = 9527;

// index.ts(14,5): error TS2540: Cannot assign to 'id' because it is a constant or a read-only property.
```

 **注意，只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候**： 

```typescript
interface Person {
    readonly id: number;
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    name: 'Tom',
    gender: 'male'
};

tom.id = 89757;

// index.ts(8,5): error TS2322: Type '{ name: string; gender: string; }' is not assignable to type 'Person'.
//   Property 'id' is missing in type '{ name: string; gender: string; }'.
// index.ts(13,5): error TS2540: Cannot assign to 'id' because it is a constant or a read-only property.
//报错信息有两处，第一处是在对 tom 进行赋值的时候，没有给 id 赋值。第二处是在给 tom.id 赋值的时候，由于它是只读属性，所以报错了。
```

### 接口继承

举个简单例子，上面我们为了结合查询功能和列表分页功能，选择将接口中某个（`name`,`teacher_birthday`）属性设置为可选，但是你可知这样做的后果有多危险？

由于`name`和`teacher_birthday`是可选的，那么有可能造成部分开发者请求时出现漏掉参数的情况

那么此刻我们可以使用接口继承来实现多个接口的组合效果

````typescript
interface ListParams {
    currentPage: number;
    pageSize: number
}

interface FilterParams extends ListParams {
    name: string;
    age: number
}


const filterParams: FilterParams = {
    currentPage: 1,
    pageSize: 5,
    name: '张三',
    age: 20
}
````

上述代码中，我们使用`FilterParams`继承了`ListParams`的接口内容，这就让`FilterParams`得到了接口`ListParams`中全部的约束内容，这让我们的代码得到了极大复用



### 只读属性

这个就是为接口某一个属性前面加上`readonly`即可，后续这个接口约束的对象中该属性的值就不允许我们修改了

````typescript
interface StuInfo {
    name:string;
    age:number;
    address:'重庆市沙坪坝区'; // 单原子类型
    readonly phone:string
}

const stuInfo:StuInfo = {
    name:'张三',
    age:20,
    address:'重庆市沙坪坝区',
    phone:'18800000000'
}

stuInfo.name = '李四'   // ok
stuInfo.phone = '18999999999'   // error:无法分配到 "phone" ，因为它是只读属性
````



### 泛型接口

有时候我们也希望接口可以复用，当我定义一个接口时，我希望某个属性可以接受多种类型，此时有两种选择：

- 高级联合类型
- 泛型接口

泛型接口的作用其实和泛型函数一样，也是将类型抽象成一个形参，当我实际使用接口的时候需要显式传入类型形参：

````typescript
interface SomeInfo {
    address:string;
    score:number
}
interface StuInfo<T,N> {
    name:N;
    age:number;
    otherData: T
}

const stuInfo:StuInfo<SomeInfo,string> = {
    name:'张三',
    age:20,
    otherData:{
        address:'重庆市沙坪坝',
        score:100
    }
}
````



### 接口扩展

我们前面尝试了接口定义，发现一个问题，所有被接口约束的对象都只能严格按照接口的结构来定义数据结构，我们无法后续添加属性，这个怎么办呢？

我们可以使用接口扩展来实现这个功能

````typescript
interface StuInfo {
    name: string;
    age: number;
    [key: string]: any	// 此处语法即为接口扩展
}

const stuInfo: StuInfo = {
    name: '张三',
    age: 20
}

// 我希望为stuInfo添加一个remarks属性
stuInfo.remarks = '你好'

const stuInfo2: StuInfo = {
    name: '李四',
    age: 21,
    score: 20
}
````

上述接口扩展有一个有趣的现象：**接口明确要求的`name`和`age`你必须书写，而扩展的属性写不写无所谓**



## Think in TS

TS最大的特点就是类型约束，可是我们如何看待类型约束呢？

- **类型约束要求我们，每一个变量都必须有明确的类型和结构**
- **类型约束也要求我们，每一个变量在参与某一行代码执行时已经明确了类型和结构**



### 类型保护

为什么需要类型保护？

因为有时候你的变量可能是高级联合类型或者是泛型这一类模糊的指代，此时如果你贸然要求变量去参与计算的话，TS编译器会提示你错误信息。

因为TS编译器无法预知当前变量最终明确的类型



但实际操作中我们经常遇到类型模糊的场景，比如：

````typescript
interface StuInfo {
    name: string;
    age: number
}

interface Stus {
    [index: number]: StuInfo
}
function fn(arg: StuInfo | Stus) {
    if(arg.length){ // error: 类型“StuInfo”上不存在属性“length”

    }
}
````

对于上述场景，我们必须先明确当前形参的类型才可以让该形参参与正常的计算过程

对于上述代码，我们必须进行明确的类型保护

所谓类型保护：**让该变量在参与计算式已经明确了类型**

对此，我们常用的方式有两种：

- 断言
- 类型谓语

断言请参考前面代码，但是断言有个弊端——每一行运算都需要断言

所以更多时候我们使用类型谓语来实现：

````typescript
interface StuInfo {
    name: string;
    age: number;
}

interface Stus extends Array<StuInfo> {
    [index: number]: StuInfo
}

function isStuInfo(arg: StuInfo | Stus): arg is StuInfo {
    return (arg as StuInfo).age !== undefined
}

function isStus(arg: StuInfo | Stus): arg is Stus {
    return (arg as Stus).length >= 0
}

function fn(arg: StuInfo | Stus) {
    if (isStuInfo(arg)) { // error: 类型“StuInfo”上不存在属性“length”
        return arg.name
    }

    if(isStus(arg)){
        return arg.length
    }

}
````





### 三目修饰&非空断言

#### 三目修饰

有时候我们定义了某个可选（或其他可能为空）的值，此时如果你不小心让这个值参与了某些非空运算场景，你也会得到错误的答案：

````typescript
interface StuInfo {
    name: string;
    age: number;
    otherData?: {
        score: number
    }
}

const stuInfo: StuInfo = {
    name: '张三',
    age: 20
}

/* 
        此处的问号是三目修饰符（可选修饰符），当前代码等价于：
        stuInfo.otherData ? stuInfo.otherData.score : undefined
    */
stuInfo.otherData?.score 
````

上述代码中`StuInfo`接口内的`otherData`是可选属性此时我们执行`stuInfo.otherData`的时候TS编译器会得出`otherData | undefined`的结论，那么接下来的`stuInfoo.otherData.score`就有报错的风险（因为`stuInfo.otherData`可能是`undefined`）

这时候我们只需要在`otherData`后面加一个`?`即可完成，这样就实现了一个三目运算的简写方案

> 就算你真的为`stuInfo`加上了`otherData`属性也是不可以的，因为TS编译器并不能知道你主观书写的代码内容，它只会按照类型约束的前后关系来执行代码检查
>
> 当然，你可以选择将`tsconfig.json`中的严格模式关掉就可以了，那这样你写锤子TS



#### 非空断言

上面为了避免`undefined`影响代码检测，我们使用了三目修饰符，其实我们还有另一条路可以选择——**非空断言**

**非空断言的意思是：开发者明确了当前变量一定有值**

非空断言的使用方式：

````typescript
stuInfo.otherData!.score 
````

## class 类

ES6提供了更接近传统语言的写法，引入了Class（类）这个概念，作为对象的模板。通过class关键字，可以定义类。基本上，ES6的class可以看作只是一个语法糖，它的绝大部分功能，ES5都可以做到，新的class写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。上面的代码用ES6的“类”改写，就是下面这样。

```typescript
class Person{
	name:strring,
	age:number,
	//在TypeScript是不允许直接在constructor 定义变量的 需要在constructor上面先声明
	constructor (name:string,age:number){
		this.name=name
        this.age=age
	}
	run(){
		
	}
}
```

### 类的修饰符

**public  private  protected**

 使用public 修饰符 可以让你定义的变量 **内部访问 也可以外部访问** 如果不写默认就是public 

![img](https://img-blog.csdnimg.cn/2eaca794296640b183cdcdddb221ee68.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAcXExMTk1NTY2MzEz,size_20,color_FFFFFF,t_70,g_se,x_16) 

 使用 private 修饰符 代表定义的变量私有的**只能在内部访问 不能在外部访问** 

![img](https://img-blog.csdnimg.cn/d64333f651ec4a799ca665cae11d0d72.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAcXExMTk1NTY2MzEz,size_20,color_FFFFFF,t_70,g_se,x_16) 

  使用 protected 修饰符 代表定义的变量私有的**只能在内部和继承的子类中访问 不能在外部访问** 

![img](https://img-blog.csdnimg.cn/5ac43616ad77488284bae4205db3f6c8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAcXExMTk1NTY2MzEz,size_20,color_FFFFFF,t_70,g_se,x_16)

### extend

extends 关键字用于类的继承。当一个类通过 extends 另一个类时，它继承了父类的所有属性和方法。子类可以覆盖（override）父类的方法以提供特定的实现。也可以通过 super 关键字调用父类的构造函数和方法。

```typescript
// 基础类
class Animal {
    name: string;
    constructor(theName: string) {
        this.name = theName;
    }
    move(distanceInMeters: number = 0) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}

// 继承自Animal的子类
class Dog extends Animal {
    constructor(name: string) {
        super(name); // 调用父类Animal的构造器
    }
    bark() {
        console.log('Woof! Woof!');
    }
}

const myDog = new Dog("Rex");
myDog.bark(); // 输出：Woof! Woof!
myDog.move(10); // 输出：Rex moved 10m.
//在这个例子中，Dog类通过extends继承了Animal类，这意味着Dog实例可以使用Animal类中定义的move方法，并且Dog类还添加了自己的方法bark
```

### implement

implements 关键字在 TypeScript 中用于确保类满足一定的接口。一个类可以实现（implement）一个或多个接口，代表它必须提供接口定义的所有属性和方法的具体实现。如果缺少了接口中定义的某个属性或方法，或者属性和方法的类型不正确，TypeScript 编译器将会报错。

```typescript
// 定义一个接口
interface IAnimal {
    name: string;
    move(): void;
}

// 实现接口的类
class Cat implements IAnimal {
    name: string;
    constructor(theName: string) {
        this.name = theName;
    }
    move() {
        console.log(`${this.name} moved stealthily...`);
    }
}

const myCat = new Cat("Whiskers");
myCat.move(); // 输出：Whiskers moved stealthily...
//在这个例子中，Cat类实现了IAnimal接口。这意味着Cat类必须定义接口中声明的name属性和move方法。如果Cat类没有遵循IAnimal接口的结构，TypeScript将会在编译时抛出错误。
```

完整例子： 

```typescript
//1. class 的基本用法 继承 和 类型约束
//2. class 的修饰符 readonly  private protected public
//3. super 原理
//4. 静态方法
//5. get set
interface Options {
    el: string | HTMLElement
}
 
interface VueCls {
    init(): void
    options: Options
}
 
interface Vnode {
    tag: string
    text?: string
    props?: {
        id?: number | string
        key?: number | string | object
    }
    children?: Vnode[]
}
 
class Dom {
    constructor() {
 
    }
 
    private createElement(el: string): HTMLElement {   //只能内部使用
        return document.createElement(el)
    }
 
    protected setText(el: Element, text: string | null) {   //给子类和内部使用
        el.textContent = text;
    }
 
    protected render(createElement: Vnode): HTMLElement {   //哪儿都能用
        const el = this.createElement(createElement.tag)
        if (createElement.children && Array.isArray(createElement.children)) {
            createElement.children.forEach(item => {
                const child = this.render(item)
                this.setText(child, item.text ?? null)
                el.appendChild(child)
            })
        } else {
            this.setText(el, createElement.text ?? null)
        }
        return el;
    }
}
 
 
 
class Vue extends Dom implements VueCls {
    options: Options
    constructor(options: Options) {
        super()   //父类的prototype.constructor.call
        this.options = options;
        this.init()
    }
 
   static version () {  //static只能调static方法和属性
      return '1.0.0'
   }
 
   public init() {
        let app = typeof this.options.el == 'string' ? 				                                 document.querySelector(this.options.el) : this.options.el;
        let data: Vnode = {
            tag: "div",
            props: {
                id: 1,
                key: 1
            },
            children: [
                {
                    tag: "div",
                    text: "子集1",
                },
                {
                    tag: "div",
                    text: "子集2"
                }
            ]
        }
        app?.appendChild(this.render(data))
        console.log(app);
 
        this.mount(app as Element)
    }
 
   public mount(app: Element) {
        document.body.append(app)
    }
}
 
const v = new Vue({
    el: "#app"
})
```

### 抽象类

 抽象类（Abstract Class） ：是一种不能被实例化的类，只能被用作其他类的基类。抽象类通常用来表示一个概念上的类，它定义了一组基本操作，这些操作必须由任何继承该抽象类的派生类来实现。 

抽象方法（Abstract Method）：是一种在抽象类中声明但不实现的方法。每个继承抽象类的派生类都必须实现这些抽象方法，否则派生类也会成为抽象类。

应用场景如果你写的类实例化之后毫无用处此时我可以把他定义为抽象类 或者你也可以把他作为一个基类-> 通过继承一个派生类去实现基类的一些方法

```typescript
//abstract 所定义的抽象类   
//abstract 所定义的方法  都只能描述不能进行一个实现
abstract class A {
   public name:string
   
}
 
new A()  //会报错  报错抽象类无法被实例化
```

我们在A类定义了 getName 抽象方法但为实现

我们B类实现了A定义的抽象方法 如不实现就不报错 **我们定义的抽象方法必须在派生类实现**

```typescript
/*
   1、如果一个类包含至少一个抽象方法，那么这个类也必须被声明为抽象的。
   2、抽象方法只能存在于抽象类中。
   3、当一个派生类继承自抽象类时，它必须实现所有的抽象方法；如果不这样做，那么派生类也必须被声明为抽象的。*/

// 抽象类
abstract class A {
  // 抽象方法
  abstract getName(): string;
}

// 派生类B继承自抽象类A，并实现了抽象方法getName
class B extends A {
  // 实现抽象方法
  getName(): string {
    return "B's name";
  }
}

// 有效的实例化，因为B实现了A中的所有抽象方法
const b = new B();
console.log(b.getName()); // 输出："B's name"

// 下面的代码将会导致编译错误，因为不能实例化抽象类
// const a = new A(); // 错误：不能创建抽象类的实例
```

## 元组

 **元组（Tuple）是固定数量的不同类型的元素的组合**。 

元组与集合的不同之处在于，元组中的元素类型可以是不同的，而且数量固定。元组的好处在于可以把多个元素作为一个单元传递。如果一个方法需要返回多个值，可以把这多个值作为元组返回，而不需要创建额外的类来表示。 

```typescript
let arr:[number,string] = [1,'string']
 
let arr2: readonly [number,boolean,string,undefined] = [1,true,'sring',undefined]

//当赋值或访问一个已知索引的元素时，会得到正确的类型：
let arr:[number,string] = [1,'string']
arr[0].length //error     //数字是没有length 的
arr[1].length //success
 
//元组类型还可以支持自定义名称和变为可选的
let a:[x:number,y?:boolean] = [1]
```

**越界元素**

```typescript
let arr:[number,string] = [1,'string']
 
arr.push(true)//error  只能push number或string类型的值
```

 对于越界的元素他的类型被限制为 联合类型（就是你在元组中定义的类型）如下图 

![img](https://img-blog.csdnimg.cn/29f23f5e7fdb43f69a6d19ee7c9c3df6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAcXExMTk1NTY2MzEz,size_20,color_FFFFFF,t_70,g_se,x_16) 

**元组和type的妙用**

```typescript
const arr:[number,boolean]=[1,false]

//取到元组里面的第一个类型也就是number
type frist=typeod arr[0]

//读到arr的length
type length=typeof arr['length']
```

## 枚举

在 TypeScript 中，枚举（Enum）是一种特殊的数据类型，它允许为一组数值赋予友好的名字。枚举的目的是为了定义一个命名的常量集合，使得代码更可读和可维护。

### 数字枚举

默认情况下，枚举是基于数字的，并且会自增。第一个枚举成员的默认值为 0，之后的成员值依次递增。

```typescript
enum Direction {
  Up,    // 0
  Down,  // 1
  Left,  // 2
  Right  // 3
}
```

你也可以手动设置枚举成员的值：

```typescript
enum Direction {
  Up = 1,
  Down,  // 2
  Left,  // 3
  Right  // 4
}
```

### 字符串枚举

枚举也可以包含字符串值，每个枚举成员必须用一个字符串字面量或另一个字符串枚举成员初始化。

```typescript
enum Direction {
  Up = "UP",
  Down = "DOWN",
  Left = "LEFT",
  Right = "RIGHT"
}
```

### 异构枚举

虽然不推荐，但 TypeScript 允许枚举包含字符串和数字成员。

```typescript
enum BooleanLikeEnum {
  No = 0,
  Yes = "YES"
}
```

### 计算的和常量成员

枚举成员可以是常量或计算出来的。一个枚举成员被视为常量：

- 如果它不带有初始化器，且它之前的枚举成员是一个数值常量。
- 如果它的值由数字字面量或其他枚举成员的引用初始化。

```typescript
enum FileAccess {
  // 常量成员
  None,
  Read    = 1 << 1,
  Write   = 1 << 2,
  ReadWrite  = Read | Write,
  // 计算的成员
  G = "123".length
}
```

### 枚举成员作为类型

特定的枚举成员可以作为类型使用，限制变量只能是枚举成员的其中之一。

```typescript
enum ShapeKind {
  Circle,
  Square,
}

interface Circle {
  kind: ShapeKind.Circle;
  radius: number;
}

interface Square {
  kind: ShapeKind.Square;
  sideLength: number;
}

let c: Circle = {
  kind: ShapeKind.Circle,
  // kind: ShapeKind.Square, // Error! Type 'ShapeKind.Square' is not assignable to type 'ShapeKind.Circle'.
  radius: 100,
}
```

### 反向映射

数字枚举成员具有反向映射，意味着你可以通过枚举的值来访问它的键。

```typescript
enum Enum {
  A
}
let a = Enum.A;
let nameOfA = Enum[a]; // "A"
```

### const 枚举

使用 const 关键字可以定义常量枚举，它们在编译阶段会被删除，并且不能包含计算成员。

```typescript
const enum Enum {
  A = 1,
  B = A * 2
}
```

在编译后的 JavaScript 代码中，常量枚举的用法会被替换为实际的数值。

枚举是 TypeScript 中强大的功能，能够让我们以类型安全的方式处理一组相关的常量值。

