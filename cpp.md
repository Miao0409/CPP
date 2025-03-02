## 2025.2.28

- class 和 struct 定义类的区别在于默认的访问权限 ，class是private，strcut是pubilc
- 封装的含义和用处
- **友元函数**：不是class的成员函数但想操作类的**非公有成员**，设置为友元函数
  friend 函数声明，但会破坏隐藏性；友元关系不存在传递性；哪怕在类中声明并且定义了友元函数，也只是**告诉编译器友元函数的访问权限**，必须在外部重新进行一次声明；

  ```
  cpp
  class Screen{
  	friend class Window_mgr;//友元类，Window_mgr的任意成员都可以访问Screen的私有部分
  	friend void Window_mgr::clear();//定义一个类的方法为另一个类的友元
  };
  ```
- **构造函数：初始化类的对象，可以是一个也可以是多个；只要类的对象被创建，就会执行对应的构造函数；不能是const成员函数；**

  - 默认构造函数
    - 如果存在默认初始值，使用初始值初始化
    - 如果没有初始值，则默认初始化
    - 只要自己定义构造函数，类就没有默认构造函数了，除非自己定义
- 拷贝、赋值、析构
- 隐式inline函数（直接在类内部实现一个函数）；显示inline函数，

  - Q:为什么要inline函数？
  - inline一般写在.h中
- 在变量声明中加入mutable，const成员可以修改mutable变量
- 浅拷贝和深拷贝：浅拷贝适合不关心对象内部数据是否共享的场景，而深拷贝适合需要完全独立数据副本的场景
- 一个const成员函数如果以引用的形式返回*this，那么他的返回类型是const引用

```cpp
const Screen& display(void) const{cout<<contents<<endl;return *this;}
```

- 类的声明
  - 前向声明 `class Screen`
  - 不完全声明，定义指针和引用；作为参数或者返回的类型
  - **类的成员变量不能是类自己**，**然而能够是自身类型的引用或者指针**否则会报错，只声明没有定义，此时为不完全类型

```cpp
class Link
{
    private:
        int a;
        Link b;//Link没有被定义，不知道所占内存空间，报错
        Link& c;//指针所占内存空间
        Link* c;//与指针相同
    public:
        Link link(Link l){};//作为参数或者返回类型，不报错
};
```

- 类的作用域
  - ：：作用域运算符
  - 编译器**处理完类的声明**后才会处理成员函数的定义
  - 先在作用域内找，之后再作用域外部找

## 2025.3.2

### 构造函数

- 初始化列表与赋值的方式 区别,底层效率问题，**重点**
  1. 如果定义了一个const成员变量，只能通过成员初始化列表
  2. 如果是引用，也必须通过成员初始化列表
  3. 另外一个没有定义默认构造函数的类成员
```cpp
class ConstRef{
  public：
      ConstRef（int n）；
  private：
      int i；
      const int ci；
      int &ri；
};
//wrong
ConstRef：：ConstRef（int n）
{
      i = n; 
      ci = 6;
      ri = ri;
}
// true
ConstRef::ConstRef(int n ):
i(n),
ci(5),
ri(ci)
{}
```
- 最好初始化顺序和定义成员变量顺序保持一致
- 默认构造函数存在两种形式,选一种就可以
  ` 1.A()=default;2.A(int wt =0,int ht=0):weight(wt),height(ht){};`
- 委托构造函数：先执行受委托构造函数体的初始化列表，再执行受委托构造函数的函数体；最后执行委托构造函数的函数体
- 构造函数作用
  - 尤其是一个类含有另一个类的类对象时，那么另外一个类对象的初始化就要靠另一个类自己的构造函数进行，如果没有对应的构造函数则会报错
### 类的隐式转换
- 隐式类型转换
  1. 通过构造函数（只有一个参数的时候）进行转换
  2. 通过类型转换运算符operator 进行转换
  3. explicit关键字阻止编译器进行隐式转换，且只有在类内部使用explicit进行声明才有效
- 显示类型转换
  1. `static_cast<Sales_data>(cin),type object = static_cast<type>(another tpye object)`
### 字面值常量类（重要 没听懂）
- 字面值类型：算术类型；引用和指针
- 字面值常量类类型：
  1. 数据成员都是字面值类型的聚合类
  2. 包含以下要求的非聚合类
     1. 数据成员都是字面值类型
     2. 类必须含有一个constexpr构造函数
     3. 的
### 类的静态成员
- 与类本身有关的成员，和类对象无关
- static 关键字
- **静态成员函数不能声明为const，也不包含this指针**
- 静态成员函数还可以通过作用域解析运算符来调用
```cpp
class Acount
{
  private:
    static double rate;
    static double rate();
  public:
    static int a;
    static int a();
};
那么可以通过：：直接访问静态成员
double r；
r = Acount：：rate（）；
也可以通过指针或者引用访问静态成员
Acount ac1；
Acount *ac2 = &ac1；
r = ac1.rate（）；
r = ac2->rate();
```
- static 关键字只在类的内部声明，若要在类外部定义函数，则不需要添加static关键字
- 绝大部分情况需要在类的外部定义和初始化静态成员，不要在类内部
- 在类内部定义常量，可以采用枚举类型和static const 类型 可以在类内部定义