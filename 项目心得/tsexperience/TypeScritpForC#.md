TypeScript For C# Developer
==
[toc]
#### 泛型
##### 1、泛型函数
```ts
function identity<T>(arg:T):T{
    return arg;
}
```
##### 2、泛型变量
eg1:
```ts
function loggingIdentity<T>(arg:T[]):T[]{
    console.log(arg.length);
    return arg;
}
```
eg2:
```ts
function loggingIdentity<T>(arg:Array<T>):Array<T>{
    console.log(arg.length);
    return arg;
}
```
##### 3、泛型类型
**泛型函数的类型**
泛型函数的类型与非泛型函数的类型没什么不同，只是有一个类型参数在最前面，像函数声明一样.
eg1:
```ts
function identity<T>(arg:T):T{
    return arg;
}
let myIdentity:<T>(arg:T)=>T=identity;
```
我们也可以使用不同的泛型参数名，只要在数量上和使用方式上能对应上就可以。
eg2:
```ts
function identity<T>(arg:T):T{
    return arg;
}
let myIdentity:<U>(arg:U)=>U=identity;
```
我们还可以使用带有调用签名的对象字面量来定义泛型函数.
eg3:
```ts
function identity<T>(arg:T):T{
    return arg;
}
let myIdentity:{<T>(arg:T):T}=identity;
```
**泛型接口**
eg1:
```ts
interface GenericIdentityFn{
    <T>(arg:T):T;
}
function identity<T>(arg:T):T{
    return arg;
}
let myIdentity:GenericIdentityFn = identity;
```
eg2:
```ts
interface GenericIdentityFn<T> {
    (arg: T): T;
}

function identity<T>(arg: T): T {
    return arg;
}

let myIdentity: GenericIdentityFn<number> = identity;
```
##### 4、泛型类
```ts
class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };
```
##### 5、泛型约束
```ts
interface Lengthwise {
    length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length); 
    return arg;
}
```
##### 6、在泛型里使用类类型
eg1:
``` ts
function create<T>(c: {new(): T; }): T {
    return new c();
}
```
eg2:
```ts
class BeeKeeper {
    hasMask: boolean;
}

class ZooKeeper {
    nametag: string;
}

class Animal {
    numLegs: number;
}

class Bee extends Animal {
    keeper: BeeKeeper;
}

class Lion extends Animal {
    keeper: ZooKeeper;
}

function createInstance<A extends Animal>(c: new () => A): A {
    return new c();
}

createInstance(Lion).keeper.nametag;  // typechecks!
createInstance(Bee).keeper.hasMask;   // typechecks!
```