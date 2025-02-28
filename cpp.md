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
-
