# learnTS

## 实时转换ts => js
1. tsc --init 生成tsconfig.json文件
2. 修改outdir "outDir": "./js"
3. 菜单栏 => 终端 => 运行任务 => typescript => 监视tsconfig.json

## ts中的类型
1. boolean : true / false
2. number
3. string
4. array
5. tuple 
元组类型用来表示已知元素数量和类型的数组，各元素的类型不必相同，对应位置的类型需要相同。
```
let x: [string, number];
x = ['Runoob', 1];    // 运行正常
x = [1, 'Runoob'];    // 报错
console.log(x[0]);    // 输出 Runoob
```
6. enum
```
enum Flag {success=200,error=201}
var f:Flag=Flag.success
console.log(f)
```
7. any
8. null undefined
```
var num:number
console.log(num) // 输出undefined 报错

var num:undefined
console.log(num) // 输出undefined 不报错
```
undefined
在 JavaScript 中, undefined 是一个没有设置值的变量。
typeof 一个没有值的变量会返回 undefined。
null
在 JavaScript 中 null 表示 "什么都没有"。

null是一个只有一个值的特殊类型。表示一个空对象引用。

用 typeof 检测 null 返回是 object。
<hr>

9. void
10. never
never 是其它类型（包括 null 和 undefined）的子类型，代表从不会出现的值。这意味着声明为 never 类型的变量只能被 never 类型所赋值，在函数中它通常表现为抛出异常或无法执行到终止点（例如无限循环）
```
let x: never;
let y: number;

// 运行错误，数字类型不能转为 never 类型
x = 123;

// 运行正确，never 类型可以赋值给 never类型
x = (()=>{ throw new Error('exception')})();

// 运行正确，never 类型可以赋值给 数字类型
y = (()=>{ throw new Error('exception')})();

// 返回值为 never 的函数可以是抛出异常的情况
function error(message: string): never {
    throw new Error(message);
}

// 返回值为 never 的函数可以是无法被执行到的终止点的情况
function loop(): never {
    while (true) {}
}
```

## 函数两种定义方式
```
function func1(): void{
    alert("func1 alert")
}
fun1()
```
```
var func2 = function():void{
    alert("func2 alert")
}
func2()
```
## ES5类
```
function Person(){
    this.name = 'zhangsan'
    this.age = 20
    this.run = function(){
        alert("zhangsan is runing")
    }
}
//原型链上方法多实例共享 构造函数不会
Person.prototype.sex = "man"
Person.prototype.work = function(){
    alert("xxx")
}
Person.getInfo = function(){
    alert("我是静态方法")
}

var p = new Person()
p.work
Person.getInfo()
```
## 对象冒充实现继承
```
function Web(){
    // 对象冒充实现继承
    Person.call(this)
}
var w = new Web()
w.run() //对象冒充可以继承构造函数里面的属性和方法
w.work() // 报错 不能继承原型链的属性和方法
```
## 原型链实现继承
```
Web.prototype = new Person() 
//原型链实现继承，可以继承构造函数里面的属性和方法也可以继承原型链的属性和方法
//存在实例化子类无法给父类构造函数传参的情况

function Person(name,age){
    this.name = name
    this.age = age
    this.run = function(){
        alert("zhangsan is runing")
    }
}
```

## 原型链+对象冒充
```
function Web(name,age){
    Person.cal(this,name,age)
}
Web.prototype = new Person()
var w = new Web('zhaosi',20)
w.run()
w.work()
```
## ts的类
```
class Person{
    name:string
    constructor(name:string){
        this.name = name
    }
    run : void {
        alert(this.name)
    }
}
var p = new Person("zhangsna")
p.run()
```
## ts实现继承 extends super
```
class Person{
    name:string
    constructor(name:string){
        this.name = name
    }
    run : void {
        alert(this.name)
    }
}
class Web extends Person{ //使用extends继承
    constructor(name:string){
        super(name) // 向上传参
    }
    run:string{
        return `${this.name} is running`
    }
}
var w = new Web('zhangsan')
w.run()
```
## 类修饰符
public：公开（默认）

protected：类内部、子类可以访问

private：类内部可以访问

## static 关键字
static 关键字用于定义类的数据成员（属性和方法）为静态的，静态成员可以直接通过类名调用。
## TypeScript 接口
接口是一系列抽象方法的声明，是一些方法特征的集合，这些方法都应该是抽象的，需要由具体的类去实现，然后第三方就可以通过这组抽象方法调用，让具体的类执行具体的方法。
```
interface IPerson { 
    firstName:string, 
    lastName:string, 
    sayHi: ()=>string 
} 
 
var customer:IPerson = { 
    firstName:"Tom",
    lastName:"Hanks", 
    sayHi: ():string =>{return "Hi there"} 
} 
 
console.log("Customer 对象 ") 
console.log(customer.firstName) 
console.log(customer.lastName) 
console.log(customer.sayHi())  
 
var employee:IPerson = { 
    firstName:"Jim",
    lastName:"Blakes", 
    sayHi: ():string =>{return "Hello!!!"} 
} 
 
console.log("Employee  对象 ") 
console.log(employee.firstName) 
console.log(employee.lastName)
```
## 加密函数式接口
```
interface encrypt{
    (key:string,value:string):string //定义了一个函数
}
//md5就必须符合接口的函数标准
var md5:encrypt = function(key:string,value:string):string{
    return key+value
}
console.log(md5("zhangsan","sili"))
```
## 可索引接口
```
interface UserArr{
    [index:number]:string
}
var array:UserArr=[123,"123"] //报错，value值必须是string类型
```
## 类类型接口，类似于抽象类
```
interface Animal{
    name:string
    eat(str:string):void
}
class Dog implements Animal{
    name:string
    constructor(name:string){
        this.name = name
    }
    eat(){
        alert(this.name+"eat food")
    }
}
```
## 泛型
any放弃了类型检查，不可以
```
// 泛型函数
function getDate<T>(value:T):T{
    return value
}
getDate<number>(5)
getDate<string>("5")
```
## 模块化
使用import和export
export default只可以使用一次，不用“{}”，import时也不用“{}”
命名空间也可以使用export暴露出来

## 装饰器(不传参)
类似java注解，不修改类的前提下提升类的功能
```
function logClass(params:any){
    console.log(params)
    params.prototype.apiURL="xxxx"
    params.prototype.run = fuction(){
        console.log("i am run method")
    }
}
@logClass
class HttpClient{
    constructor(){

    }
    getData(){

    }
}
var c = new HttpClient()
console.log(c.apiURL)
c.run()
```
## 装饰器工厂(传参)
```
function logClass(params:string){
    return function(target:any){
        console.log(target)
        console.log(params)
        target.prototype.apiURL=params
    }
}
@logClass("hello")
class HttpClient{
    constructor(){

    }
    getData(){

    }
}
var c = new HttpClient()
console.log(c.apiURL)
```
```
function logClass(target:string){
    console.log(target)
    return class extend target{

        apiURL=this.apiURL+"修改后"
        getDate(){
            console.log(this.apiURL)
        }
    }
}
@logClass("hello")
class HttpClient{
    constructor(){

    }
    getData(){
        console.log(this.apiURL)
    }
}
var c = new HttpClient()
console.log(c.apiURL)
```
