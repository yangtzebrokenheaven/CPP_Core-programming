# 1.内存分区模型

CPP程序在执行时，将内存大方向划分为4个区域

·代码区：存放函数体的二进制代码，由操作系统进行管理的

·全局区：存放全局变量和静态变量以及常量

·栈区：由编译器自动分配释放，存放函数的参数值，局部变量等

·堆区：由程序员分配和释放，若不释放，程序结束时由操作系统回收

## 1.1程序运行前

程序编译后，生成可执行exe文件，未执行该程序前分为两个区域

**代码区：**

存放cpu的机器指令

共享，只读

**全局区**

全局变量和静态变量，常量存放在此

```cp
#include <iostream>
#include <string>
using namespace std;
int a_b = 10;
int a_C = 10;//全局变量
int main()
{
	int a = 10;
	int b = 10;//局部变量
	cout<<(int)&a<<endl;
	cout<<(int)&b<<endl;
	cout<<(int)&a_C<<endl;
	cout<<(int)&a_b<<endl;
	//静态变量
	static int s_a = 10;//全局变量和静态变量放在一起
}
```

字符串常量“”

const修饰的变量

const分为修饰局部和全局

##  1.2栈区

存放函数参数值，局部变量

不返回局部变量的地址

```cp
#include <iostream>
#include <string>
using namespace std;
int * func()
{
	int a = 10;
	return &a;
}
int main()
{
	int *p = func();
	cout<<*p<<endl;//第一次会正确返回
	cout<<*p<<endl;//第二次就会释放无法返回
}
```

## 1.3堆区

由程序员分配释放，若不释放程序结束后由操作系统回收

主要利用new开堆区开辟内存

```cp
#include <iostream>
#include <string>
using namespace std;
int * func()
{
	int *p = new int(10);
	return p;
}
int main()
{
	int *p = func();
	cout<<*p<<endl;//第一次会正确返回
	cout<<*p<<endl;//第二次就会释放无法返回
}
```

## 1.4new操作符

开辟new，释放delete

#  2.1引用

作用：给变量起别名

数据类型 &别名 = 原名

## 2.2引用注意事项

·引用必须初始化

·初始化后不可以更改

## 2.3引用做函数参数

让形参修饰实参

```Cp
//交换函数
void Swap(int &a,int &b)
{
	int temp = a;
	a = b;
	b =temp;
}
```

## 2.4引用做函数返回值

引用是可以作为函数的返回值

1. 不要返回局部变量的引用，函数的调用可以作为左值

   ```cpp
   int& twst01()
   {
       static int a  =10;
       return a;
   }
   int main()
   {
       int &r = twst01();
       cout<<r<<endl;
       twst01() = 100;
       cout<<r<<endl;
   }
   ```

如果函数的返回值是引用，则这个函数可以作为左值

## 2.5引用的本质

引用的本质在CPP中内部就是一个指针常量

```cpp
int main(){
    int a = 10;
    int& ref = a;//自动转换为int * const ref = &a
}
```

## 2.6常量引用

修饰形参，防止误操作

```cp
int a = 10;
int & ref = a;//正确
int & ref = 10；//错误
const int &ref = 10；//正确
```

```cpp
void fun(int & a)
{
    int a = 100;
    cout << a << endl;
}
int main ()
{
    int a = 10;
    fun(a);//输出100
    cout << a <<endl;//输出100，原a的值已被修改
}
```

# 3.函数进阶

## 3.1函数默认参数

若函数中有多个参数，但是在调用时传入参数个数过少会报错，传入参数的值优先级大于默认参数

- 如果某个位置有了默认参数，那么从这个位置往后，从左到右必须有默认值

- 如果函数声明有默认参数，该函数实现就不能有默认参数

## 3.2函数占位参数

 函数中的形参列表里可以有占位参数，带哦用函数时必须填补该位置

```cp
返回值类型 函数名（数据类型）{}
```

## 3.3函数重载

函数名可以相同，提高复用性

个数不同，类型不同，位置不同

- 引用作为重载条件

```cpp
void func(int &a)
{
    cout << “a1” <<endl;
}
void func(const int &a)
{
    cout << “a2” << endl;
}
int main()
{
    int a = 10;
    func(a);//调用    cout << “a1” <<endl;
    func(10);//cout << “a2” << endl;
}
```

- 重载碰到默认参数

```cpp
void func(int a,int b = 10)
{
    cout << “a,b” <<endl;
}
void func(int a)
{
    cout << “a,b” <<endl;
}
int main()
{
    int a = 10;
    func(10);//出现二义性，报错
}
```

# 4.类和对象

## 4.1封装

CPP三大特性之一

- 将属性和行为作为一个高增提，表现生活中的事物
- 将属性和行为加以权限控制

**封装的意义一**

​	在设计类的时候，属性和行为写在一起，表现

语法	 ` clsss 类名{访问权限 ： 属性/行为}；`

案例：设计一个圆类，求圆的周长

```cpp
#include <iostream>
#include <string>
using namespace std;
const double PI = 3.14;
class Circle 
{
public:
	int m;
	double backlen()
	{
		return 2 * PI * m;
	}
};
int main()
{
	Circle c1;
	c1.m = 10;
	cout << c1.backlen() << endl;
}
```

## 4.2封装案例

**设计一个学生类，属性有姓名和学号，可以给姓名和学号赋值，可以显示学生的姓名和学号**

```cp
#include <iostream>
#include <string>
using namespace std;
const double PI = 3.14;
class student
{
public:
	int id;
	string name;
};
int main()
{
	student c1;
	cin >> c1.id;
	cin >> c1.name;
	cout << c1.id << c1.name<< endl;
}
```

## 4.3封装权限

public       	类内可以访问，类外可以访问

protected 	类内可以访问，类外不可以访问

private		 类内可以访问，类外不可以访问

## 4.4struct和class区别

struct和class区别在于默认的访问权限不同

struct默认为public，class默认权限为private

## 4.5成员属性设置为私有

将所有成员属性设置为私有，可以自己控制读写权限

```cpp
#include <iostream>
#include <string>
using namespace std;
class Person{
    public:
    void setname(string name)//可读可写
    {
        N_name = name;
    }
    string getname()
    {
        return N_name;
    }
    int getage()//可读不可写
    {
        return A_age;
    }
    private:
    int A_age = 0;
    string N_name;
}；
int main()
{
	    Person p;
    	p.setname("MIKE");
    	cout << "Name is :" << getname << endl;
    	
}
```

## 4.6对象的初始化和清理

### 4.6.1构造函数和析构函数

自己不提供构造函数和析构函数，编译器会自动提供和调用

- 构造函数：创建对象时为对象的成员属性赋值
- 析构函数：用于再对象销毁前系统自动调用，执行一些清理工作

**构造函数语法：**

` 类名（）{}`

1. 构造函数没有返回值也不写void
2. 函数名称和类型相同
3. 可以有参数，可以发生重载
4. 程序再调用对象时会自动调用构造，无需手动调用

**析构函数语法：**

` ~类名（）{}`

1. 不可以有参数
2. 销毁时调用

```cpp
//对象的初始化和清理
class Person
{
public:
    Person()
    {
        cout<<"构造函数调用"<<endl;
    }
    ~Person()
    {
        cout<<"析构函数调用"<<endl;
    }
};
int main()
{
    Person p;
    return 0;
}
```

### 4.6.2构造函数的分类和调用

有参构造和无参构造

普通构造和拷贝构造

```cpp
class Person
{
    Person()
    {
         cout<<"构造函数调用"<<endl;
    }//无参构造
     Person()
    {
         cout<<"构造函数调用"<<endl;
    }//有参构造
    Person(const Person &p)
    {
        //将传入的对象的属性拷贝
    }//拷贝构造函数
}
//调用
void  test()
{
    //1.括号法
    Person p;
    Person p1(10);//调用有参
    Person p3(p2);//调用拷贝    
    //2.显示法
    Person p4;
    Person P5 = Person(10)
    //3.隐式转换法
    Person p6 = 10//Person p6 = Person p6(10)
}
```

###  4.6.3拷贝构造函数的调用时机

- 使用一个已经创建完毕的对象来初始化一个新对象
- 值传递的方式给函数参数传值
- 以值方式返回局部对象

```cpp
class Person
{
    Person()
    {
         cout<<"构造函数调用"<<endl;
    }
        Person(int age)
    {
         cout<<"有参构造函数调用"<<endl;
       Aage = age;
    }
     ~Person()
    {
         cout<<"析构函数调用"<<endl;
    }
    Person(const Person &p)
    {
        cout<<"拷贝构造函数函数调用"<<endl;
        Aage = p.Aage;
    }
    int Aage;
}
void test01()
{
    Person p1(20);
    Person p2(p1);
}
void doWork(Person p)
{
    
}
void test02()
{
    Person p;
    doWork(p);
}
```

输出结果为：

![image-20230318182457684](C:\Users\34844\Desktop\笔记\Cpp_Day2\image-20230318182457684.png)

### 4.6.4构造函数调用规则

规则如下

- 用户自定义构造函数，不提供默认构造函数，但会提供默认拷贝构造
- 自定义拷贝函数，不提供其他构造函数

### 4.6.5深拷贝和浅拷贝

深拷贝：在堆区重写申请空间，进行拷贝操作

浅拷贝：简单的赋值拷贝操作

```cpp
class Person
{
    Person()
    {
         cout<<"构造函数调用"<<endl;
    }
        Person(int age,int height)
    {
         cout<<"有参构造函数调用"<<endl;
         h_height = new int(height);
         Aage = age;
    }
     ~Person()
    {
         cout<<"析构函数调用"<<endl;//释放堆区开辟的数据
         if(h_height != NULL)
         {
             delete h_height;
             h_height = NULL;//堆区内存重复释放
         }
    }
    Person(const Person &p)
    {
        cout << "拷贝构造函数调用"<<endl;
        Aage = p.Aage;
        h_height = new int(*h_height);
    }
    int m_age;
    int *h_heigt;
}
void test()
{
    Person p1(18);
    Person p2(p1);//浅拷贝
}
```

### 4.6.6初始化列表

`构造函数（）：属性1（）值1`

```cpp
class Person
{
public:
    Person(int a,int b,int c):m_a(a),m_b(b),m_c(c)
    {
        
    }
    int m_a;
    int m_b;
    int m_c;
}
```

### 4.6.7类对象作为类成员

成员可以是另一个类的对象，称为对象成员

```cpp
class A{}
class B
{
    A a;
}
```

```cpp
class phone
{
    public:
    phone (string name)
    {
        pname = name;
    }
    string pname;
};
class Person
{
    public:
    Person(string name1,string pname1):name(name1),pname
(pname1)
    {
    }
    string name;
    phone m_phone;
};
```

先调用类成员，在调用该类

析构函数调用为先进后出，后进先出

### 4.6.8静态成员

- 静态成员变量
  1. 所有对象共享同一份数据
  2. 编译阶段分配内存
  3. 类内声明，类外初始化

- 静态成员函数

  1. 所有对象共享同一个函数

  2. 静态成员函数只能访问静态成员变量

     **静态成员变量**

```cpp
class Person
{
    public:
    static int m_A;
    
};
int Person::m_A = 100;
void test01()
{
    Person p1;
};
//静态成员不属于某个对象上
//静态成员变量也有访问权限，私有作用域依然受到限制
```

**静态成员函数**

```cpp
class Person
{
    public:
    static int m_A;
    static void func()
    {
        cout << "sadasd " <<endl;
    }
};//调用和变量相同
```

