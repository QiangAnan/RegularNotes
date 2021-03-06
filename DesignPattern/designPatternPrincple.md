## 设计模式之禅
在继续看后面的设计模式大pk之前，我需要对前面的知识进行梳理，对比以及形成自己的一套说辞<br>
先来整理一下设计模式的首要的几个原则。一共 **六大**
### 一、单一职责原则
一句很牛❌哄哄的英文是这样说的 "There should never be more than one reason for a class to change"<br>
有且仅有一个原因引起类的变更。(这里用到的数学引语也是很伟大，有且仅有！有且仅有，就是只有唯一的一个原因可以引起变化)<br>
举个🌰吧!<br>
现代人喜欢吃饭，睡觉，打豆豆。
```cpp
class IEat{
    virtual void eat() = 0;
};
class ISleep{
    virtual void sleep() = 0;
};
class IDadd{
    virtual void dadd() = 0;
};
```
三个接口分别实现吃饭睡觉打豆豆，每个行为都是只有一个变化引起，而且对外公布的是接口，而不是具体的实现类，所以因为具有单一原则，我们可以实现<br>
* 让类的复杂性降低，而不是让一个类很牛逼。<br>
* 让可读性提高，而不是天书一样<br>
* 可维护性提高，而不是没有spanner，都不能修。<br>
* 变更引起的风险低，变的是单一接口，所以，不会有连锁反应。<br>
人生有多少个十年，这个十年，就做一件事情。<br>
所以，在写程序的时候，多考虑一下，让一个类或者一个方法只做一件事情。<br>
### 二、里氏替换原则（我是一个有原则的类）
**is-a**。<br>
定义：所有引用基类的地方必须能透明地使用其子类的对象。<br>
爸爸在你就可以在，你在爸爸不可以在(特别是在做羞羞的事情的时候>_<!!!>);青出于蓝而胜于蓝，所以爸爸在的地方，你可以适应，你在的地方，爸爸未必适应。<br>
里氏替换原则四层定义。<br>
* 子类必须完全实现父类的方法。<br>
* 子类可以有自己的个性。(仔大仔世界)<br>
* 覆盖或实现父类的方法时输入参数可以被放大。<br>
子类中的方法的前置条件必须与超类中被覆写的方法的前置条件相同或者更宽松。<br>
* 覆写或实现父类的方法时输出结果可以被缩小。<br>
其实里氏替换原则就是为了增强程序的健壮性的，传递不同的子类实现不同的方法。<br>
所以重载的时候，应该让子类的参数更加宽松。<br>
一言毙之日:扑朔迷离!

### 依赖倒置原则
高层模块不应该依赖低层模块，两者都应该依赖其抽象。<br>
抽象不应该依赖细节<br>
细节应该依赖抽象<br>
就是面向接口编程，不要面向细节，不面向业务。<br>
实现类之间不要产生什么直接依赖关系，通过接口或抽象类产生。<br>
接口或抽象类不依赖实现类。<br>
实现类依赖接口或抽象类。<br>
🌈 就是面向对象，面向接口编程！<br>
提高代码的可读性和可维护性。减少类之间的耦合性，提高系统的稳定性，降低并行开发引起的风险，提高代码的可读性和可维护性。<br>
```cpp
class IDriver{
    void drive(ICar car);
};

class Driver:private IDriver{ // 实现这个方法为主
    void drive(Icar car);
};

class ICar{
    void run();
};

class Benz:private ICar{
    void run();
};
class BWM:private Icar{
    void run();
};

int main(){
    IDriver driver = new Driver();
    Icar benz = new Benz();
    driver.drive(benz); // 传入不同的参数，显式不同的输出
}
```
测试的时候，直接新建一个接口，就可以测试！简直无敌。<br>
#### 依赖的写法
* 构造函数传递依赖对象<br>
这个叫构造函数注入。这个不用多讲，就是构造函数传参传入。<br>
* setter方法传递依赖对象<br>
```cpp
class IDriver{
    void setCar(Icar car);
};
class Driver:private IDriver{
    private ICar iicar;
    void setCar(Icar icar){
        this.iicar = icar;
    }
};
```
* 接口声明依赖对象<br>
此上的第二个🌰就是接口声明依赖对象。<br>
其实依赖倒置，就是通过抽象或接口使各个类或模块的实现彼此独立，不相互影响，实现模块间的松耦合。<br>
有五个原则需要在项目中守约（守约，今天的长城也很.....）<br>
* 每个类尽量都有接口或抽象类，或者抽象类和接口两者具备<br>
* 变量的表面类型尽量是接口或者抽象类。
* 任何类都不应该从具体类派生（派生抽象类吧）
* 尽量不要覆写基类的方法。
* 结合里氏替换原则使用 is-a
### 接口隔离原则（客户端不应该依赖它不需要的接口&类间的依赖关系应该建立在最小的接口上）
建立单一的接口，不要建立臃肿庞大的接口。<br>
接口尽量细化，同时接口中的方法尽量少。<br>
不要和上面的单一混淆了！单一职责就是职责单一，职责！接口隔离原则，就是你的接口方法尽量少。<br>
**麻雀虽小，五脏俱全** 定义的接口尽量比较小，但是也要涵盖具体的业务。所以，接口小也要满足单一职责的原则。<br>
* 一般来说，接口一个只服务于一个子模块或业务逻辑
* 通过业务逻辑压缩public方法，自己内部使用private就好，这样也能实现高内聚（犀利）
* 了解环境，不要盲从。不是叫你小，你就一定小，接口拆分的标准要按你的业务逻辑来。<br>
### 迪米特法则(最少知识原则) 保持神秘感，知道太多，会惹来杀身之祸的😜！
一个对象应该对其他对象有最少的了解。<br>
一个类应该对自己需要耦合或调用的类知道得最少，你内在跟我没关系，我只需要知道你提供的接口就可以了。<br>
Only talk to your immediate friends<br>
### 开闭原则(对修改关闭，对扩展开放) 
尽量扩展以实现变化，而不是修改实现。<br>
```cpp
class IBook{
    
};
class NoteBook:public IBook{

};
class OffNoteBook:public NoteBook{

};// 只需要扩展 就可以实现不同的变化
```
#### 对象约束
对于抽象要有约束，尽量不能修改，只是扩展，然后让后面的类继续继承，然后扩展。<br>
* 通过接口或抽象类约束扩展，对扩展进行边界限定，不允许出现在接口或抽象类中不存在的public方法
* 参数类型、引用对象尽量使用接口或抽象类，而不是实现类
* 让抽象层保持稳定，一旦确定即不允许修改
```cpp
list<IBook> lisBook; // 使用抽象类
```
#### 元数据控制模块行为
使用数据，就是类似，配置文件，一旦确定基本不用修改。<br>
#### 制定项目章程
约定优于配置<br>
#### 封装变化
* 将相同的变化封装到一个接口或抽象类中
* 将不同的变化封装到不同的接口或抽象类中，不应该有两个不同的变化出翔在同一个接口或抽象类中。<br>
封装可能的变化，具体可以继续查看后面的23个设计模式，怎样封装变化。<br>
六大原则结合起来SOLLID（solid 稳定）很稳。<br>
拥抱变化，让这六大原则伴随23个设计模式，继续乘风破浪，直挂云帆。(真有文化！)<br>

