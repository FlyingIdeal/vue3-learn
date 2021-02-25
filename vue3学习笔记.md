## 第二章：Typescript讲解



#### 最新的ECMAScript定义了8种数据类型

- 7种原始类型
  - Boolean
  - Null
  - Undefined
  - Number
  - BigInt
  - String
  - Symbol
- 1种引用类型
  - Object



#### 在ts中各种原始类型如何命名

1. Boolean

   ```
   let isDone: boolean = false
   
   // 如果修改为 isDone = 123 // 此时会报错
   ```

2. Number 和 String

   ```
   let age: number = 30
   
   let name: string = '小明'
   ```

3.  undefined 和 null

   ```
   let unde: undefined = undefined
   let nu: null = null
   ```

   

4.  允许创建任意类型的变量 any

   ```
   let noSure: any = 4
   noSure = '我能变化成字符串' // 此时不会报错
   
   同时也可变化成对象
   例如：
   noSure.myName = 'Fly'
   noSure.getName()
   ```



#### 数组和元组

1. 数组

   ```
   let arr: number[] = [1, 2, 4] // 数字类型的数组
   
   // arr.push('sss')  报错
   
   
   let str: string[] = ['a', 'b', 'c']  //字符串类型的数组
   ```

2. 元组  

   ```
   let user: [number, string] = [24, '小明'] // 有限制，必须一一对应，第一个number必须数字，第二个字符串也要对应
   
   user.push(12)
   user.push('daming')
   
   // 上面两个不会报错，可以push，也不会报错，但仅限push这两种数据类型 
   
   ```

#### 类型推断

1. 混合元素

```
let numOrStr: number | string // 不确定数字或者字符串类型

numOrStr = 123
numOrStr = 'abc'



// 写个函数  union types
function getLen(input: string | number): number {
	const str = input as string
	if(str.length) {
		return str.length
	} else {
		const num = input as number
		return num.toString().length
	}
}

// 优化上面的代码  type guard
function getLen(input: string | number): number {
	if (typeof input === 'string') {
		return input.length
	} else {
		return input.toString().length
	}
}
```



#### Interface接口（用于定义js中的对象 Object）

 <!-- ps:不存在js中，在ts编译时不会被转换过去，只能用于对象类型的静态检查-->

   ```
interface Person {
    readonly id: number; // readonly只读属性，在定义时创建，后续不可更改
    name: string; // ps: 实际运行中末尾的; 可以是, 也可以不填写
    age: number;
    degree?: string; // 学位，字符串类型，加上?意思为可选可不选
}

// 此时声明一个对象，必须遵循Person的命名规则
let xiaoming: Person = {
    id: 1,
    name: 'xiaoming', // ps: 此处必须为逗号,
    age: 20
}

// 声明一个degree为?的变量
let daming: Person = {
    id: 2
    name: 'daming',
    age: 12,
    degree: '本科'
}


// 验证readonly
// daming.id = 123 // 报错
   ```

#### Function函数

```
// 约定x,y都为数字，返回的也是数字类型; z为可选数字类型
function add(x:number, y:number, z?:number): number { 
	return x + y
}

// 下面这种写法和上面相同，函数表达式写法，只是展示箭头函数 => 写法，为add2铺路
//const add(x:number, y:number, z?:number): number => {
//	return x + y
//}

add(1, 2)


// 如果赋值给另外一个函数变量，则必须满足相同规则
let add2:(x:number, y:number, z?:number) => number = add //返回number类型的函数
```



```
ps： 展示interface的魔力，可以约定函数

// 把上一个代码块改编
interface iSum {
	(x:number, y:number, z?:number): number
}

let add2: iSum = add
```

#### 类 Class

- 类（Class）: 定义了一切事物的抽象特点
- 对象（Object）: 类的实列，通过new 创建
- 面向对象（OOP）三大特征： 封装、继承、多态

```
// 类在es6中的写法

// 1. 封装：暴露方法给外部使用
class Animal {
	constructor(name) {
		this.name = name
	}
	run() {
		return `${this.name} in running`
	}
}

const snake = new Animal('snake')
console.log(snake.run())



// 2. 继承

class Dog extends Animal {
	bark() {
		return `${this.name} is barking` // 狂吠
	}
}

const dogA = new Dog('dogA')
console.log(dogA.run())
console.log(dogA.dark())



// 3. 多态

class Cat extends Animal {
	static categories = ['mammal'] // 可以直接访问的属性
	constructor(name, age) {
		super(name)
		this.age = age
	}
	run() {
		return `重新run方法，并且还能调用父类的方法， ${super.run()}`
	}
}

console.log(Cat.categories) // ['mammal']
```

#### Typescript 中的类

- Public : 修饰的属性或方法是共有的
- Private: 修饰的属性或方法是私有的
- Protected: 修饰的属性或方法是受保护的 （意思是只有一个家族的使用，例如父亲或者是继承的子类）

```
// TS中class的书写
class Animal {
	readonly name: string; // 只读
	constructor(name) {
		this.name = name
	}
	run() {
		return `${this.name} is running`
	}
}

const snake = new Animal('snake')
// snake.name = 'aaa'  // 此时会报错，因为是只读
```

#### 类和接口(接口又名方法)

 <!--类可以使用 implements来实现接口-->

```
// 约定公共方法
interface Radio {
	switchRadio(trigger: boolean): void {	// void 代表啥都不返回
		
	}
}

// 约定公共方法
intreface Battery { // 电池容量
	checkBatteryStatus(): void
}

// 创建一个Car的类，其中需要保证有Radio中的方法
class Car implements Radio {
	switchRadio(trigger: boolean){}
}

// 同之上的道理
class Cellphone implements Radio, Battery { // 此处只需要用逗号分隔即可
	switchRadio(trigger: boolean){}
	checkBatteryStatus() {} // 手机有检查电池容量的方法
}
```



#### 枚举（enums）

<!--任何项目中都能用到常量，常量指在执行过程中不会改变的值，js中一般用const声明一个常量，但是有些取值是在一系列范围内，比如说一周内的七天，三颜色（红黄蓝），四个方位（上下左右），这些值就可以用枚举来表示-->

```
// 1. 数字枚举
enums Direction {
	Up,
	Down,
	Left,
	Right
}

console.log(Direction.Up) // 默认为0
console.log(Direction[0]) // 也回输出0
// 因为enums有个默认映射值  Direction[Direction['Up'] = 0] = "Up"


// 2. 常量枚举
const enum Direction {
	Up: 10
}
// 加入const 就成了常量枚举
```



#### 泛型（Generics）、、T

```
// 1. 举个例子， 泛型的写法


// T是神秘变量，现在是什么类型并不知道，可以是任意类型，在使用的时候给它指定即可，
function echo<T>(arg: T): T {
	return arg
}

const result = echo('str') // 此时传入的是string类型，所以返回的类型也是string

// 写一个错误的返回
const result2: string = echo(true) // 此时传入的true为boolean类型，但是约定返回是string，会报错
```



```
// 2. 交换数组的两个内容

//  E => 元素集合的元素类型； K和V => 关键字和值的类型； T => 需要时的类型； U和S => 任意类型 

function swap<T, U>(tuple: <T, U>): [U, T] {
	return [tuple[1], tuple[0]]
}
const result = swap('string', 123)
```

######  

#### 约束泛型

```
1. 泛型举例

此时不知道arg是否有类型返回，如果不做约束，编译时会报错
function echoArrLen<T>(arg: T): T {
	console.log(arg.length) // 此时编译会报错，并不是所有的数据类型都有length属性
	return arg.length
}
const arr = echoArrLen() // 例如，参数为空的情况下，并没有length属性



2. 把上面例子修改，约定有length属性

// 此种方法还是有弊端，比如只能传入数组，但是string也有length属性

functon echoArrLen<T>(arg: T[]): T[] { // 修改入参和返回，都加上[]，做约束
	return arg.length
}
const arr = echoArrLen([1, 2, 3])


```

```
// 3. 针对以上两种方法，可以使用interface形式，约束

// 约定方法必须有length属性
interface IWidthLen {
	length: number 
}

function echoWidthLength<T extends IWidthLen>(arg: T): T {
	return arg.length
}

const str = echoWidthLength('str')
const obj = echoWidthLength({length: 10})
const arr = echoWidthLength([1, 2, 3])

```

#### 泛型在其他方面的应用



1. 泛型在类方面的应用

```
class Queue<T> { // 约束为T类型
	private data: [];
	push(item: T) { // push可以是任意类型
		this.data.push(item)
	}
	pop(): T { // 吐出任意类型
		this.data.shift()
	}
}

const queue = new Queue<number>() // 因为新new出来的类，约定只能是number类型，所有后续push('str')报错
queue.push(12)
queue.push('str') // 此时回报错，因为约定的是数组类型


```

1. 泛型在iinterface中的使用

```
interface KeyPair<T, U> {
	key: T,
	value: U
}

let keypair1: KeyPair<number, string> = {key: 1, value: 'str'}
let keypair2: KeyPair<string, number> = {key: 'str', value: 2}
```

1. 泛型在数组中的使用

```
let arr: number[] = [1, 2, 3]

可以改编为下面这种方式

let arrTwo: Array<number> = [1, 2, 3]
```



#### 类型别名和字面量和交叉类型



1. 类型别名： type aliase

   ```
   // 如果每次命名都有(x: number, y: number)这一大长串，会显得很冗余难书写
   
   let sum:(x: number, y: number): number => {
   	return x + y
   }
   const resulet = sum(2, 3)
   
   
   可以使用type 定义一种约束
   
   // 优化上述方法
   type PlusType = (x: number, y: number): number => // 提起公共部分
   
   let sum2: PlusType
   
   const resulet = sum2(2, 3)
   
   
   ```

   ```
   // 同理，可以在数字或字符串中使用
   type strOrNumber = string | number
   let result2:strOrNumber = 'str'
   
   result2 = 123
   ```

2. 字面量

   ```
   let str: 'name' = 'name' // 只能命名为name
   let num: 1 = 1 // 只能命名为1
   
   type Direction = 'Up' | 'Down' | 'Left' | 'Right'
   let toWhere:Direction = 'Up' // 只能命名上行代码其中一个值
   ```

   

3. 交叉类型  ---   &

```
// 使用 & 符号进行链接，能组合多种类型

interfacce IName {
	name: string
}

type IPerson = IName & {age: number} // 此时IPerson 具备了name和age两种属性

let person: IPerson = {name: 'xiaoming', age: 23}
```



#### 声明文件 declare



```
// 使用场景

比如在ts文件中用到了jQuery，但是ts文件无法识别，导致报错，所以这时候用 declare 说明

仅仅用于编译时的检查，并不是实现功能的真正代码



declare var jQuery:(selection: string) => any
jQuery('"#foo"')


```



#### 内置类型 built-in



1. global  object

   ```
   比如  Array
   
   let a: Array<number> = 123
   
   同理， String, Date, Reg...等等
   ```

   

2. build-in object

   ```
   Math.pow(2, 2) // 默认生成的约定
   
   // 此时Math ==> An intrinsic object that provides basic mathmeatics functionality and constants（提供基本数学功能和常量的内在对象）
   ```

   

3. DOM and BOM

   ```
   let body = document.body //此时 document: Document;  body:HTMLElment 对象
   ```

   

4. Utility Types (实用类型)

```
Partial<T> 可选类型

Omit<T, K> 忽略类型

还有很多其他内置类型，可参考官网api
```



## 第三章：vue3.0， 新特性讲解

#### 新特性巡礼

1. 性能提升
   - 打包减少41%
   - 初次渲染快55%，更新快133%
   - 内存使用减少54%
2. Compositon API
   -  ref 和 reactive
   - computed 和 watch
   - 新的生命周期函数
   - 自定义函数  ---   Hooks函数
3. 其他新增属性
   - Teleport - 瞬移组件的位置
   - Suspense - 异步加载组件的新福音
   - 全局API 的修改和优化 
4. 更好的Typescript支持

#### 配置vue-cli

```
1. 全局安装： npm install -g @vue/cli
2. vue ui // 打开可视图
```

#### ref的妙用

```
<template>
	<div class="home">
		<h2>{{count}}</h2>
		<h2>{{double}}</h2>
		<button @click="add">增加++</button>

		<h6>{{rNumber}}</h6>
		<button @click="add2">reactive增加++</button>
	</div>
</template>

<script lang="ts">
import { ref, computed, reactive, toRefs } from 'vue'

export default {
	setup() {
		const count = ref(0) // 生产ref响应式对象
		const double = computed(() => {
			return count.value * 2
		})
		const add = () => {
			return count.value++
		}


		// 生成reactive对象，然后再用toRefs格式化
		const reData = reactive({
			rNumber: 100,
			add2: () => {
				reData.rNumber++
			}
		})

		return {
			count,
			double,
			add,
			...toRefs(reData)
		}
	}
}
</script>

```

#### vue3中响应式和vue2中的区别

```
// vue2中： 对于数组和对象，是重构的函数
Object.defineProperty(data, 'count', {
	get() {},
	set() {}
})

// 而vue3中，通过es7中的 new Proxy
new Proxy(data, {
	get(key) {},
	set(key, value) {}
})
```

#### 生命周期

###### 生命周期图示

![](http://m.qpic.cn/psc?/V11p5mQv2iuIp0/TmEUgtj9EK6.7V8ajmQrEMrv9*sHrOLAx.EDmak*HiQEvD0aXUox1TnIMY8BlMA4PM0ydw48XJGgstHw2zR2Y.MhYg2QSd6Plyl*WmlDhy8!/b&bo=OARoCAAAAAADRz4!&rf=viewer_4)

###### vue3和vue2钩子函数对比

![](http://m.qpic.cn/psc?/V11p5mQv2iuIp0/TmEUgtj9EK6.7V8ajmQrELWb7OsXbqnijilvwisEpa79peLdpJRUy8O3FJUtM2*JWUKxKVCU7wgtTS4lV*4ruc4SOC2TlzGA5eSJU.j44CE!/b&bo=1ASaAgAAAAABF3g!&rf=viewer_4)

