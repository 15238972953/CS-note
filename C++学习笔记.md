### 1.C/C++指针初始化问题

c++中的指针是一个很经典的用法，但是也是最容易出错的，比如**定义了一个指针，必须对其进行初始化**，不然这个指针指向的是一个未知的内存地址，后续对其操作的时候，会报错。这只是其次，最让人头疼的就是指针错误问题，往往编译的时候可以通过，在程序运行的时候，就会出现异常，如果对程序不是很熟悉，则不是很容易找到问题所在。

### 2.VS提示scanf不安全问题

scanf()函数是标准C中提供的标准输入函数，用以用户输入数据

scanf_s()函数是Microsoft公司VS开发工具提供的一个功能相同的安全标准输入函数，从vc++2005开始，VS系统提供了scanf_s()。在调用该函数时，必须提供一个数字以表明最多读取多少位字符。

scanf()在读取数据时不检查边界，所以可能会造成内存访问越界：

解决方法：在代码第一行加上下面的代码

`#define _CRT_SECURE_NO_WARNINGS`

### 3.字符串常量

```c
char *p="hello";
// p[0]='H';   不可以修改字符串常量区，只读的，不可以用strcpy
//指针直接指向的是字符串常量区，所以之后不可以对其进行修改；

char str[]="hello";
char *p=str;
p[0]='H';
//此时是可以修改的，这里是字符串数组，因为此时字符串是存放在栈区
```



### 4.const（指针常量和常量指针）

在C语言中：

```c
const int a = 20;  //可以不初始化
//不能定义数组长度
int arr[a];  //错误的
// a是一个常变量，不能作为左值，但是可以被修改,比如下面的方式：
int* p = (int*)&a;
*p = 30;
printf("%d %d %d\n", a, *p, *(&a));
```



在C++中：

`const`修饰必须初始化，叫做**常量**；

```c++
const int a = 10; //必须初始化
//在C++中所有出现a的地方都会在编译阶段被10替代；

int a=10;
const int b = a;   //这里又变成常变量了，而不是常量；因为初始值不是立即数，而是一个变量；
```

**c和c++中const的区别是什么?**

const的编译方式不同，c中，const就是当作一个变量来编译生成指令的；

C++中，所有出现const常量名字的地方，都被常量的初始化替换了!!



const 修饰 char *p 时，指针指向的空间不可修改，而指针p是可以修改的；

```c
char str[]="hello";
const char *p;
p=str;
```

const修饰p时，指针p不可以修改，指针指向的空间可以修改；

```c
char str[]="hello";
char *const p=str;
p[0]='H';
// p=str1; 错误；
```

```c
const int a;     //指的是a是一个常量，不允许修改。
const int *a;    //a指针所指向的内存里的值不变，即（*a）不变
int const *a;    //同const int *a;
int *const a;    //a指针所指向的内存地址不变，即a不变
const int *const a;   //都不变，即（*a）不变，a也不变
```

### 5.数组指针和指针数组

[(136条消息) 指针数组与数组指针详解_men_wen的博客-CSDN博客_指针数组和数组指针](https://blog.csdn.net/men_wen/article/details/52694069?ops_request_misc=%7B%22request%5Fid%22%3A%22167248483116800184145903%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=167248483116800184145903&biz_id=0&spm=1018.2226.3001.4187)

### 6.二级指针



### 7.函数指针和指针函数

1. **定义不同**
   指针函数本质是一个函数，其返回值为指针。
   函数指针本质是一个指针，其指向一个函数。

2. **写法不同**

   ```c
   指针函数：int *fun(int x,int y);
   函数指针：int (*fun)(int x,int y);
   ```

```c
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>

void b()
{
	printf("I am a func b\n");
}

void a(void (*p)())
{
	printf("I am a func a\n");
	p();
}

//函数名中村的就是函数的入口地址，如果要存储函数地址，需要函数指针；
int main()
{
	void (*p)(); //p就是一个函数指针
	p = b;
	a(b);
	return 0;
}
```

函数指针的**应用场景**：**回调**（callback）。我们调用别人提供的 API函数(Application Programming Interface,应用程序编程接口)，称为Call；如果别人的库里面调用我们的函数，就叫Callback。

```c
//指针函数示例
typedef struct _Data{
    int a;
    int b;
}Data;
//指针函数
Data* f(int a,int b){
    Data * data = new Data;
    //...
    return data;
}
int main(){
    //调用指针函数
    Data * myData = f(4,5);
    //Data * myData = static_cast<Data*>(f(4,5));
   //...
}

//函数指针示例
int add(int x,int y){
    return x+y;
}
//函数指针
int (*fun)(int x,int y);
//赋值
fun = add;
//调用
cout << "(*fun)(1,2) = " << (*fun)(1,2) ;
//输出结果
//(*fun)(1,2) =  3
```

### 8.协程技术



### 9.递归

递归调用深度过大时，会出现stack overflow；



### 10.全局变量

全局变量不可以写到头文件里面；

注意**static**、**extern**的使用；

1. static :
   - 局部变量：将变量存储在静态变量区，只初始化一次，函数执行结束存储的值不会释放；
   - 全局变量：不能被其他文件借用；
   - 函数：其他文件中不能调用这个函数；
2. extern ：在本函数中借用其他文件的全局变量；



一旦局部变量和全局变量重名**就近原则**；



### 11.setjmp

可以从当前函数跳转到任意函数



### 12.结构体指针

```c
#include<stdio.h>
struct student
{
    char name[20];
    int num;
    char sex;
};
int main()
{
    struct student s1 = { "lele",101,'nan' }; 	
    struct student* p;
    p = &s1;
    //下面三种等价；
    printf("%s %d %c\n", s1.name, s1.num, s1.sex);
    printf("%s %d %c\n", p->name, p->num, p->sex);
    printf("%s %d %c\n", (*p).name, (*p).num, (*p).sex);
    
    struct student arr[3] = { "xiaoming",102,'nan',"lizi",105,'nan',"cuihua",104,'nv' };
	p = arr;
    int num;
    num = p->num++;
    printf("%d %d\n", num, p->num); 	//102 103
    num = p++->num;
    printf("%d %d\n", num, p->num); 	//102 105
}
```



C语言中，定义一个结构体，必须写成struct student s1，在C++中，可以写成student s1;



### 13.计数排序

```c
#include<stdio.h>

void count(int *nums)
{
	int count[100] = { 0 };
	int i, j, k;
	for (i = 0; i < 10; i++)
	{
		count[nums[i]]++;
	}
	k = 0;
	for (i = 0; i < 100; i++)
	{
		for (j = 0; j < count[i]; j++)
		{
			nums[k++] = i;
		}
	}
}

int main()
{
	int nums[] = { 23,54,37,83,25,85,63,95,37,55 };
	count(nums);
	for (int i = 0; i < 10; i++)
	{
		printf("%3d", nums[i]);
	}
}
```



### 14.红黑树



### 15.qsort函数

```c
#include <stdio.h>
#include <stdlib.h>

int compare(const void* a, const void* b)
{
    return (*(int*)a - *(int*)b);
}

int main()
{
    int nums[] = { 88, 56, 100, 2, 25 };
    for (int i = 0; i < 5; i++) 
        printf("%d ", nums[i]);

    qsort(nums, 5, sizeof(int), compare);

    for (int i = 0; i < 5; i++)
        printf("%d ", nums[i]);
    return(0);
}
```



### 16.动态数组



```c
#include <stdio.h>
#include <stdlib.h>
//动态数组的写法
int main()
{
    int* a = (int*)malloc(sizeof(int) * 5);
    for (int i = 0; i < 5; i++)
    {
        a[i] = i;
    }
    a = (int*)realloc(a,sizeof(int) * 10);
    return(0);
}
```



### 17.scanf标准输入原理

scanf缓冲区为空时，scanf才会卡住，

```c
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
int main()
{
	int a;
	char b;
	scanf("%d", &a);
	printf("a=%d\n", a);
	scanf("%c", &b);
	printf("b=%c\n", b);
	return 0;
}
```



### 18.scanf循环读取

实现每输入一个，就输出结果，便于测试；

```c
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
int main()
{
	int a;
	while ( scanf("%d", &a) != EOF)
	{
		for (int i = 0; i < a; i++)
		{
			printf("*");
		}
		printf("\n");
	}
	return 0;
}
```



### 19.string转char

```c++
//错误
string s[]= {"h","e","l","l","o"};
vector<char> str={s->begin(),s->end()};

//错误
vector<string> s={"h","e","l","l","o"};
vector<char> str={s.begin(),s.end()};

```



### 20.reverse反转

reverse(a,b);

a，b都是迭代器类型或者说是指针类型；

a指向待反转的第一个位置，b指向待反转的最后一个位置的**后一位**；

```c++
//字符串的反转
string s;
reverse(s.begin(),s.end());

//数组的反转
int nums[10];
reverse(nums,nums+10);
```



### 21.resize()函数

```c++
string s;
s.resize(20);	//重新将s的大小设置为20；
```

resize：对某个vector容器调用resize方法会在该容器尾部添加（初始化为0）或删除一些元素，使容器达到指定的大小。

reserve: 对某个vector容器调用reserve方法仅仅设置capacity这个值。

### 22.多态的基本概念及虚函数

**多态是C++面向对象三大特性之一**

多态分为两类

* 静态（编译时期）多态: **函数重载** 和 **运算符重载**属于静态多态，复用函数名；模版（函数模版、类模版）
* 动态（运行时期）多态: **派生类和虚函数实现运行时多态；具体来说，基类函数为虚函数，派生类中对这个函数进行了重写，并且在外部基类指针（引用）指向派生类对象，通过该指针（引用）调用同名覆盖方法；基类指针指向哪个派生类对象，就会调用哪个派生类对象的同名覆盖方法，称为多态；**
* **多态底层是通过动态绑定来实现的；pb指针（引用）访问谁的vfptr，继续访问谁的vftable,就调用对应的派生类对象方法；**



静态多态和动态多态区别：

* 静态多态的函数地址早绑定  -  编译阶段确定函数地址
* 动态多态的函数地址晚绑定  -  运行阶段确定函数地址



**虚函数：**虚函数的作用是允许在派生类中重新定义与基类同名的函数，并且可以通过基类指针或者引用来访问基和派生类中的同名函数。

常见的不能声明为虚函数的有：普通函数（非成员函数），静态成员函数，内联成员函数，构造函数，友元函数；

![image-20250608193924065](.\picture\image-20250608193924065.png)

**总结一:**

一个类里面定义了虚函数，那么编译阶段，编译器给这个类类型产生一个唯一的vftable虚函数表，虚函数表中主要存储的内容就是**RTTI指针**和**虚函数的地址**。当程序运行时，每一张虚函数表都会加载到内存的.rodata区；

**总结二:**

一个类里面定义了函数数，那么这个类定义的对象，其运行时，内存中开始部分，多存储一个vfptr虚函数指针，指向相应类型的虚函数表vftable。一个类定义的多个对象，他们的vfptr指向的都是同一张虚函数表；

**总结三：**

一个类里面虚函数的个数，不影响对象内存大小(vfptr)，影响的是虚函数表的大小；

**总结四：**

如果派生类中的方法，和基类继承来的某个方法，返回值、函数名、参数列表都相同而且基类的方法是virtua1虚函数，那么派生类的这个方法，自动处理成虚函数；

构造函数不能成为虚函数；

static静态成员方法不能成为虚函数，因为静态方法并不依赖对象；



下面通过案例进行讲解多态

```C++
class Animal
{
public:
	//Speak函数就是虚函数
	//函数前面加上virtual关键字，变成虚函数，那么编译器在编译的时候就不能确定函数调用了。
	virtual void speak()
	{
		cout << "动物在说话" << endl;
	}
};

class Cat :public Animal
{
public:
	void speak()
	{
		cout << "小猫在说话" << endl;
	}
};

class Dog :public Animal
{
public:

	void speak()
	{
		cout << "小狗在说话" << endl;
	}

};
//我们希望传入什么对象，那么就调用什么对象的函数
//如果函数地址在编译阶段就能确定，那么静态联编
//如果函数地址在运行阶段才能确定，就是动态联编

void DoSpeak(Animal & animal)
{
	animal.speak();
}
//
//多态满足条件： 
//1、有继承关系
//2、子类重写父类中的虚函数
//多态使用：
//父类指针或引用指向子类对象

void test01()
{
	Cat cat;
	DoSpeak(cat);


	Dog dog;
	DoSpeak(dog);
}


int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：

多态满足条件

* 有继承关系
* 子类重写父类中的虚函数

多态使用条件

* 父类指针或引用指向子类对象

重写：函数返回值类型  函数名 参数列表 完全一致称为重写



### 23.纯虚函数和抽象类

在多态中，通常父类中虚函数的实现是毫无意义的，主要都是调用子类重写的内容；

因此可以将虚函数改为**纯虚函数**；

纯虚函数语法：`virtual 返回值类型 函数名 （参数列表）= 0 ;`

当类中有了纯虚函数，这个类也称为==抽象类==；

**抽象类特点**：

 * 无法实例化对象，但可以定义指针和引用变量；
 * 子类必须重写抽象类中的纯虚函数，否则也属于抽象类；

**示例：**

```c++
class Base
{
public:
	//纯虚函数
	//类中只要有一个纯虚函数就称为抽象类
	//抽象类无法实例化对象
	//子类必须重写父类中的纯虚函数，否则也属于抽象类
	virtual void func() = 0;
};

class Son :public Base
{
public:
	virtual void func() 
	{
		cout << "func调用" << endl;
	};
};

void test01()
{
	Base * base = NULL;
	//base = new Base; // 错误，抽象类无法实例化对象
	base = new Son;
	base->func();
	delete base;//记得销毁
}

int main() {

	test01();

	system("pause");

	return 0;
}
```



基类中调用派生类中重写的纯虚函数；

```c++
#include <iostream>
using namespace std;

class Base {
public:
    virtual void call() = 0;

    void invoke() {
        cout << "Base::invoke()" << endl;
        call();  // ✅ 这里是虚函数调用，会触发动态绑定
    }

    virtual ~Base() = default;
};

class Derived : public Base {
public:
    void call() override {
        cout << "Derived::call()" << endl;
    }
};

int main() {
    Derived d;
    Base* p = &d;

    p->invoke();  // 会调用 Derived::call()
}
```



### 24.at函数

访问vector中的数据，使用两种方法来访问；

1. vector::at()
2. vector::operator[]

operator[]主要是为了与C语言兼容，他可以像C语言数组一样操作。但at()是我们的首选，因为at()进行了边界检查，如果访问超过了vector的范围，将抛出一个异常。由于operator[]容易造成一些错误，所以我们很少用它；



### 25.交换函数

```c++
void change(int *p)
{
	int temp=p[0];
	p[0]=p[1];
	p[1]=temp;
}

int main()
{
	int arr[2]={10,20};
	change(arr);
}
```



```C++
void change(int *p)
{
    p[0]=p[0]^p[1];		// ^ 按位异或运算
    p[1]=p[1]^p[0];
    p[0]=p[0]^p[1];
}

int main()
{
	int arr[2]={10,20};
	change(arr);
    cout<<arr[0]<<" "<<arr[1]<<endl;
}
```

### 26.常见的三种哈希结构

- 数组
- set（集合）
- map（映射）

set：

| 集合          | 底层实现 | 是否有序 | 数值是否可以重复 | 能否更改数值 | 查询效率  | 增删效率  |
| ------------- | -------- | -------- | ---------------- | ------------ | --------- | --------- |
| set           | 红黑树   | 有序     | 否               | 否           | O(log(n)) | O(log(n)) |
| multiset      | 红黑树   | 有序     | 是               | 否           | O(log(n)) | O(log(n)) |
| unordered_set | 哈希表   | 无序     | 否               | 否           | O(log(1)) | O(log(1)) |

std::unordered_set底层实现为哈希表，std::set 和std::multiset 的底层实现是红黑树，红黑树是一种平衡二叉搜索树，所以key值是有序的，但key不可以修改，改动key值会导致整棵树的错乱，所以只能删除和增加。

| 映射          | 底层实现 | 是否有序 | 数值是否可以重复 | 能否更改数值 | 查询效率  | 增删效率  |
| ------------- | -------- | -------- | ---------------- | ------------ | --------- | --------- |
| map           | 红黑树   | key有序  | key不可重复      | key不可修改  | O(log(n)) | O(log(n)) |
| multimap      | 红黑树   | key有序  | key可重复        | key不可修改  | O(log(n)) | O(log(n)) |
| unordered_map | 哈希表   | key无序  | key不可重复      | key不可修改  | O(log(1)) | O(log(1)) |

std::unordered_map 底层实现为哈希表，std::map 和std::multimap 的底层实现是红黑树。同理，std::map 和std::multimap 的key也是有序的



### 27.迭代器和下标之间转换

转换主要是使用stl中的advance和distance函数来进行的，
**advance**是将iterator移动指定个元素，**distance**是计算两个iterator直接的距离。

distance计算第一个参数到第二个参数之间的距离。如果第二个参数的顺序在第一个参数前面的话,函数是会返回负值的；如果迭代器不在一个容器内,程序会抛出异常。

advance是将第一个参数向后移动第二个参数指定个元素。如果第二个参数为负,则向前移动；如果向前或向后移动超出容器范围,则抛出异常。



### 28.substr（）

**substr有2种用法：**

```c++
string s = "0123456789";
string sub1 =s.substr(5);  //只有一个数字5表示从下标为5开始一直到结尾：sub1 = "56789"
string sub2 = s.substr(5, 3);	//从下标为5开始截取长度为3位：sub2 = "567"
```



### 29.指数函数

```c++
pow(2,3);  //2^3
```



### 30.左值引用和右值引用

**引用是一种更安全的指针；**

**引用必须初始化，指针可以不初始化；**

```c++
int &a=20;  //错误的，因为20无法取地址
```

引用只有以及引用，，没有多级引用；

在C++中，左值引用和右值引用都是引用类型，它们的主要区别在于它们能够绑定的表达式类型不同。

左值引用（lvalue reference）可以绑定到一个左值（lvalue）表达式上，例如具有名称的变量、具有内存地址的表达式、返回左值引用的函数等等。左值引用的声明方式是在类型前加上一个“&”符号，例如int&。

右值引用（rvalue reference）可以绑定到一个右值（rvalue）表达式上，例如字面量、临时对象、返回右值引用的函数等等。右值引用的声明方式是在类型前加上两个“&&”符号，例如int&&。

右值引用的引入主要是为了实现移动语义和完美转发。移动语义是指在一些情况下，将一个对象的资源转移给另一个对象，可以避免不必要的复制和销毁操作，从而提高程序的性能。完美转发是指在模板编程中，将参数按照原来的类型和值传递给另一个函数，避免类型转换和拷贝构造函数的调用。

需要注意的是，一个左值引用可以绑定到一个左值或一个左值引用上，但不能绑定到一个右值引用上；一个右值引用只能绑定到一个右值或一个右值引用上，但不能绑定到一个左值引用上。

- 左值引用：引用一个对象；
- 右值引用：就是必须绑定到右值的引用，C++11中右值引用可以实现“移动语义”，通过 && 获得右值引用。

```cpp
int x = 6; // x是左值，6是右值
int &y = x; // 左值引用，y引用x

int &z1 = x * 6; // 错误，x*6是一个右值
const int &z2 =  x * 6; // 正确，可以将一个const引用绑定到一个右值

int &&z3 = x * 6; // 正确，右值引用
int &&z4 = x; // 错误，x是一个左值
```



### 31.unordered_map实现原理

unordered_map是C++ STL中的一个关联容器，它使用哈希表来存储键值对，可以提供O(1)的常数时间的插入、查找和删除操作。

unordered_map的实现原理可以简单概括为以下几个步骤：

1. 哈希函数：unordered_map使用哈希函数将键映射到桶（bucket）中，每个桶中存储一个链表或红黑树，用于解决哈希冲突。

2. 桶的分配：unordered_map会在创建时分配一定数量的桶，当插入元素时，会根据哈希函数计算出键所在的桶，并将该元素插入到桶中。

3. 冲突解决：当两个或多个键映射到同一个桶时，就会发生哈希冲突。unordered_map使用链表或红黑树来解决这种冲突，当链表长度到达一定阈值时，会转换为红黑树来提高查找效率。

4. 内存管理：unordered_map使用内存池来管理内存，可以提高插入和删除操作的效率，避免频繁的内存分配和释放操作。

总之，unordered_map的实现原理主要是基于哈希表的思想，使用哈希函数将键映射到桶中，并使用链表或红黑树来解决哈希冲突，同时使用内存池来管理内存。这些技术使得unordered_map能够提供高效的插入、查找和删除操作，是C++ STL中常用的容器之一。

### 32.智能指针

### 33.二级指针

问：

```C++
int **m=new (int)[1];
    m[0]=new int[6];
    delete[] m[0];
```

这段二级指针的代码有什么问题吗？

答：

这段二级指针的代码存在内存泄漏的问题，需要进行改正。

首先，使用new运算符创建二级指针时，应该使用双层括号语法，即new int*[1]，而不是new (int)[1]。因为new int*[1]可以创建一个指向指针的指针，而new (int)[1]只会创建一个整型数组，并返回指向该数组第一个元素的指针。

因此，可以将代码修改为：

```C++
int **m = new int*[1];
m[0] = new int[6];
delete[] m[0];
delete[] m;
```

这段代码首先使用new int*[1]创建了一个指向指针的指针m，并使用m[0] = new int[6]创建了一个包含6个整型变量的动态数组，并将该数组的首地址存储在m[0]中。然后，使用delete[] m[0]释放了该数组所占用的内存。最后，使用delete[] m释放了指向指针的指针m所占用的内存。

需要注意的是，在使用new运算符创建动态数组时，应该使用delete[]运算符释放数组所占用的内存，而不是使用delete运算符。否则，会导致内存泄漏或程序崩溃的问题。

### 34.delete []p;和delete p;区别

`delete []p; `和 `delete p; `是用于释放动态分配内存的 C++ 运算符，它们之间的区别在于：

1. delete[] p; 用于释放由 new[] 运算符动态分配的数组内存。

例如，如果你使用了 new[] 运算符来动态分配一个整型数组，那么在释放该数组内存时，应该使用 delete[] 运算符：

```c++
int *p = new int[10];
// 使用 p 操作数组
delete[] p; // 释放 p 指向的整型数组内存
```

注意：使用 delete[] 运算符释放动态分配的数组时，必须使用 [] 语法来指明释放的是一个数组。

2. delete p; 用于释放由 new 运算符动态分配的单个对象内存。

例如，如果你使用了 new 运算符来动态分配一个整型对象，那么在释放该对象内存时，应该使用 delete 运算符：

```c++
int *p = new int;
// 使用 p 操作对象
delete p; // 释放 p 指向的整型对象内存
```

注意：使用 delete 运算符释放动态分配的单个对象时，不能使用 [] 语法。

总之，使用 delete[] 运算符释放动态分配的数组内存，使用 delete 运算符释放动态分配的单个对象内存，这两个运算符的用法必须正确匹配，否则会**导致内存泄漏或者程序崩溃**等问题。

### 35.NULL和nullptr区别

NULL和nullptr都是C++中用于表示空指针的关键字，它们之间的区别在于：

1. 类型不同

NULL是C++早期版本中用来表示空指针的宏定义，它被定义为整型常量0。因此，NULL实际上是一个整型常量而非指针类型，这意味着它可以被隐式转换为任何指针类型。

nullptr是C++11中引入的新特性，它是一个关键字，表示空指针，类型为std::nullptr_t。因此，nullptr是一个真正的指针类型，而不是整型常量。

2. 安全性不同

NULL在C++早期版本中被广泛使用，但它有一个缺陷，即它可以被隐式转换为任何指针类型，这可能会导致一些安全问题。例如，如果一个函数参数是指针类型，但是传入NULL，编译器可能会将NULL隐式转换为指针类型，这可能会导致程序运行时出现未定义的行为。

nullptr解决了上述安全问题，因为它只能被转换为指针类型，而不能被转换为其他类型。如果尝试将nullptr隐式转换为其他类型，编译器会产生错误。

总之，nullptr是C++11中引入的新特性，它是一个真正的指针类型，比NULL更加安全和可靠。如果你正在使用C++11或更高版本，建议使用nullptr来表示空指针，以提高代码的安全性和可读性。

### 36.内联函数

内联函数是一种在编译时将函数调用展开为函数体的优化手段，它的作用是提高程序的执行效率。内联函数的定义通常放在头文件中，因为它在每个调用点上都被展开，所以需要在每个使用该函数的文件中都有该函数的定义。

内联函数的特点如下：

1. 函数体简单

内联函数的函数体通常比较简单，因为如果函数体过于复杂，展开后会使代码变得冗长，反而会影响程序的执行效率。

1. 频繁调用

内联函数通常被用于频繁调用的函数，因为函数调用会产生一定的开销，如果使用内联函数，可以减少函数调用的开销，提高程序的执行效率。

1. 代码量小

内联函数通常适用于函数体较小的函数，因为函数体较小的函数展开后不会使代码变得过于冗长，同时也不会占用太多的内存空间。

内联函数的优点在于它可以减少函数调用的开销，提高程序的执行效率，但是它的缺点在于会增加代码的体积，因为每个调用点上都会展开函数体，所以如果使用不当，会导致代码变得过于冗长，反而会影响程序的执行效率。因此，应该根据实际情况来使用内联函数，避免过度使用。

在编译时没有函数调用的开销，在函数调用点直接把函数的代码展开处理；inline函数不在生成函数符号；

inline只是建议编译器把这个函数处理成内联函数，但不是所有的inline都会被处理为内联函数，如递归函数和函数内部代码较多，

debug下inline是不起作用的，只有在release下才起作用；

### 37.i++和++i区别

1. **效率不同**：后置++执行速度比前置的慢；

2. **i++ 不能作为左值，而++i 可以**：

   ```c
   int i = 0;
   int *p1 = &（++i）；//正确
   int *p2 = &（i++）；//错误
   ++i = 1；//正确
   i++ = 1；//错误
   ```

   

3. 两者都不是原子操作；++ i 是先加后赋值；i ++ 是先赋值后加；++i和i++都是分两步完成的。

### 38.new和malloc的区别

1. new是操作符，而malloc是函数。
2. new在调用的时候先分配内存，在调用构造函数，释放的时候调用析构函数；而malloc没有构造函数和析构函数。
3. malloc需要给定申请内存的大小，返回的指针需要强转；new会调用构造函数，不用指定内存的大小，返回指针不用强转。
4. new可以被重载；malloc不行
5. new分配内存更直接和安全。
6. new发生错误抛出异常，malloc返回null

**底层实现：**

**malloc底层实现：**当开辟的空间小于 128K 时，调用 brk（）函数；当开辟的空间大于 128K 时，调用mmap（）。malloc采用的是内存池的管理方式，以减少内存碎片。先申请大块内存作为堆区，然后将堆区分为多个内存块。当用户申请内存时，直接从堆区分配一块合适的空闲快。采用隐式链表将所有空闲块，每一个空闲块记录了一个未分配的、连续的内存地址。

**new底层实现：**关键字new在调用构造函数的时候实际上进行了如下的几个步骤：

1. 创建一个新的对象
2. 将构造函数的作用域赋值给这个新的对象（因此this指向了这个新的对象）
3. 执行构造函数中的代码（为这个新对象添加属性）
4. 返回新对象

```C++
int a = 0;
int *p = new (&a) int(20);    //在指定的a地址内存填充20；
```

new会进行初始化，而malloc不会初始化；

### 39.const和define的区别

const用于定义常量；而define用于定义宏，而宏也可以用于定义常量。都用于常量定义时，它们的区别有：

1. const生效于编译的阶段；define生效于预处理阶段。
2. const定义的常量，在C语言中是存储在内存中、需要额外的内存空间的；define定义的常量，运行时是直接的操作数，并不会存放在内存中。
3. const定义的常量是带类型的；define定义的常量不带类型。因此define定义的常量不利于类型检查。

### 40.简述C++的内存管理

![image-20201219142935577](https://static.nowcoder.com/images/activity/2021jxy/c/assert/2.png)

1. **内存分配方式**：

   在C++中，内存分成5个区，他们分别是堆、栈、自由存储区、全局/静态存储区和常量存储区。

   **栈**:在执行函数时，函数内局部变量的存储单元都可以在栈上创建，函数执行结束时这些存储单元自动被释放。

   **堆**:就是那些由new分配的内存块，一般一个new就要对应一个delete。

   **自由存储区**:就是那些由malloc等分配的内存块，和堆是十分相似的，不过是用free来结束自己的生命。

   **全局/静态存储区**:全局变量和静态变量被分配到同一块内存中

   **常量存储区**:这是一块比较特殊的存储区，里面存放的是常量，不允许修改。

2. **常见的内存错误及其对策**：

   （1）内存分配未成功，却使用了它。

   （2）内存分配虽然成功，但是尚未初始化就引用它。

   （3）内存分配成功并且已经初始化，但操作越过了内存的边界。

   （4）忘记了释放内存，造成内存泄露。

   （5）释放了内存却继续使用它。

   对策：

   （1）定义指针时，先初始化为NULL。

   （2）用malloc或new申请内存之后，应该**立即检查**指针值是否为NULL。防止使用指针值为NULL的内存。

   （3）不要忘记为数组和动态内存**赋初值**。防止将未被初始化的内存作为右值使用。

   （4）避免数字或指针的下标**越界**，特别要当心发生“多1”或者“少1”操作

   （5）动态内存的申请与释放必须配对，防止**内存泄漏**

   （6）用free或delete释放了内存之后，立即将指针**设置为NULL**，防止“野指针”

   （7）使用智能指针。

3. **内存泄露及解决办法**：

   **什么是内存泄露？**

   简单地说就是申请了一块内存空间，使用完毕后没有释放掉。（1）new和malloc申请资源使用后，没有用delete和free释放；（2）子类继承父类时，父类析构函数不是虚函数。（3）Windows句柄资源使用后没有释放。

   **怎么检测？**

   第一：良好的编码习惯，使用了内存分配的函数，一旦使用完毕,要记得使用其相应的函数释放掉。

   第二：将分配的内存的指针以链表的形式自行管理，使用完毕之后从链表中删除，程序结束时可检查改链表。

   第三：使用智能指针。

   第四：一些常见的工具插件，如ccmalloc、Dmalloc、Leaky、Valgrind等等。

### 41. C 语言如何实现 C++ 语言中的重载

**函数重载：**一组函数称得上重载，一定是在同一个作用域内；

​					函数重载与返回值无关；

C++可以重载，C不可以：因为C++重载是看**函数名加形参列表**，C语言只看**函数名**；

c语言中不允许有同名函数，因为编译时函数命名是一样的，不像c++会添加参数类型和返回类型作为函数编译后的名称，进而实现重载。如果要用c语言显现函数重载，可通过以下方式来实现：

1. 使用函数指针来实现，重载的函数不能使用同名称，只是类似的实现了函数重载功能
2. 重载函数使用可变参数，方式如打开文件open函数
3. gcc有内置函数，程序使用编译函数可以实现函数重载

示例如下：

```c
#include<stdio.h>

void func_int(void * a)
{
    printf("%d\n",*(int*)a);  //输出int类型，注意 void * 转化为int
}

void func_double(void * b)
{
    printf("%.2f\n",*(double*)b);
}

typedef void (*ptr)(void *);  //typedef申明一个函数指针

void c_func(ptr p,void *param)
{
     p(param);                //调用对应函数
}

int main()
{
    int a = 23;
    double b = 23.23;
    c_func(func_int,&a);
    c_func(func_double,&b);
    return 0;
}
```

typedef void (*Fun) (void) 的理解:

```c
typedef char (*PTRFUN)(int); 
PTRFUN pFun; 
char glFun(int a)
{
    return;
} 
void main() 
{ 
    pFun = glFun; 
    (*pFun)(2); 
}
```

 typedef的功能是定义新的类型。第一句就是定义了一种PTRFUN的类型，并定义这种类型为指向某种函数的指针，这种函数以一个int为参数并返回char类型。后面就可以像使用int,char一样使用PTRFUN了。

### 42.容器的空间配置器（allocator）

STL 容器（如 `vector`, `list`, `map`）内部不直接用 `new` 和 `delete` 管理内存，而是通过 **allocator（配置器）** 接口来统一完成**内存申请 + 对象构造 + 内存释放 + 析构**四步。

### 43.友元函数

**友元函数（friend function）** 是一种可以访问类的私有（`private`）和保护（`protected`）成员的**非成员函数**。它不是类的成员，却能像成员函数一样访问类的内部。在写运算符重载函数时比较常用；

```c++
class MyClass {
private:
    int data;

public:
    MyClass(int d) : data(d) {}

    // 声明友元函数
    friend void printData(const MyClass& obj);
};

void printData(const MyClass& obj) {
    std::cout << "数据是：" << obj.data << std::endl;
}

```

### 44.深拷贝与浅拷贝

**浅拷贝**：仅复制指针的值（地址），多个对象共享同一块内存。

**深拷贝**：复制指针指向的实际数据，两个对象互不干扰。

```c++
class MyClass {
public:
    int* data;

    MyClass(int val) {
        data = new int(val);
    }

    // 浅拷贝构造函数（默认生成）
    MyClass(const MyClass& other) {
        data = other.data;  // 只是复制了指针
    }

    // 深拷贝构造函数（手动实现）
    // MyClass(const MyClass& other) {
    //     data = new int(*other.data);  // 拷贝内容
    // }

    ~MyClass() {
        delete data;
    }
};

int main(){
    MyClass a(10);
    MyClass b = a;   // 执行浅拷贝（默认行为）

    *a.data = 20;
    std::cout << *b.data << std::endl;  // 输出 20，不是期望的 10！

    // 当 a 和 b 析构时，两次 delete data —— 崩溃（野指针）
}
```

### 45.迭代器失效问题？

迭代器失效（Iterator Invalidated）是指原来指向容器中某元素的迭代器，在某些操作（如插入、删除或容器重分配）后**变得不可用或指向错误位置**，继续使用会导致 **未定义行为（crash、错值、逻辑错误）**。

```c++
//情况一
std::vector<int> v = {1, 2, 3};
auto it = v.begin();
v.push_back(4);  // 如果扩容，it 会失效！

//情况二
v.insert(it,4); // 插入行为，会使it之后的所有迭代器都失效；
//正确做法
it = v.insert(it,4);

//情况三
v.erase(it)   //删除行为，会使it之后的所有迭代器都失效；
//正确做法
it = v.erase(it);
```

==在vector中的erase，删除一个元素后，后面的元素都往前移动一个位置，那迭代器不是应该还可以指向vector元素吗，为什么还会失效?==

`erase` 操作确实会导致被删除元素之后的所有元素向前移动，但**迭代器失效的根本原因并不是元素移动本身**，而是因为 `vector` 的内存管理机制和标准库的规范设计。**可能会重新分配内存；**

- **元素移动 ≠ 迭代器有效**：虽然元素向前移动，但迭代器的本质是**指向内存地址的指针或封装对象**。当元素移动后，原迭代器指向的地址可能已经存储了其他数据或变成非法地址。
- **标准库的强制规定**：C++ 标准明确要求 `erase` 操作会使指向被删除元素及之后所有元素的迭代器失效，这是为了统一行为并避免隐式错误。

即使某些实现中迭代器物理上仍指向有效元素（如新移动过来的元素），标准库仍规定它**逻辑上失效**，因为：

- **一致性要求**：不同编译器的实现可能不同（如是否原地覆盖或重新分配内存），标准统一规定失效可避免依赖实现细节。
- **安全性**：防止开发者误用迭代器导致跨平台的未定义行为。

### 46.继承

| 继承方式  | 基类的访问限定 | 派生类的访问限定 | 外部的访问限定 |
| --------- | -------------- | ---------------- | -------------- |
| public    | public         | public           | Y              |
| public    | protected      | protected        | N              |
| public    | private        | 不可见的         | N              |
| protected | public         | protected        | N              |
| protected | protected      | protected        | N              |
| protected | private        | 不可见的         | N              |
| private   | public         | private          | N              |
| private   | protected      | private          | N              |
| private   | private        | 不可见的         | N              |

### 47.继承中的隐藏关系

在派生类中，如果定义了一个与基类**同名但参数列表不同**的成员函数或变量，它会**隐藏**（遮蔽）基类中所有**同名**成员函数或变量（不管参数列表是否相同）。

```c++
#include <iostream>
using namespace std;

class Base {
public:
    void show() {
        cout << "Base::show()" << endl;
    }

    void show(int x) {
        cout << "Base::show(int)" << endl;
    }
};

class Derived : public Base {
public:
    using Base::show;   // 方法二：引入所有 Base::show 重载版本
    void show() {
        cout << "Derived::show()" << endl;
    }
};

int main() {
    Derived d;
    d.show();       // ✅ 调用 Derived::show()
    // d.show(1);   // ❌ 编译错误：Base::show(int) 被隐藏了
    d.Base::show(1);  //方法一：可以这样调用
}
```

**同名函数与变量混合时优先隐藏函数**

```c++
class Base {
public:
    void foo() { }
};

class Derived : public Base {
public:
    int foo;  // 隐藏 Base::foo() 函数
};

```

### 48.基类与派生类转换

在继承结构中进行上下的类型转换，默认只支持从下到上的类型的转换；

```c++
int main(){
	Derived d(10);
	Base b(20);
	b=d;   // ✅派生类对象赋给基类对象    类型从下到上的转换
	d=b;   // ❌基类对象赋给派生类对象    类型从上到下的转换
	Base *pb=&d;    // ✅基类指针《-派生类   类型从下到上的转换
	Derived *pd=&b; // ✅派生类指针《-基类   类型从上到下的转换
}
```

### 49.继承中的覆盖关系

结合第22条

![image-20250608195457780](./picture/image-20250608195457780.png)

派生类中定义了一个函数，它与基类中的**虚函数**具有**相同的函数签名**（包括函数名、参数类型、参数个数、const 修饰等），此时派生类的函数将**覆盖**基类的虚函数，派生类的这个方法，自动处理成虚函数；即为**重写虚函数**

虚函数表中基类虚函数的地址派生类虚函数地址被覆盖；

```c++
#include <iostream>
using namespace std;

class Base {
public:
    virtual void speak() const {
        cout << "Base speaking" << endl;
    }
};

class Derived : public Base {
public:
    void speak() const override {  // 完全一致，构成覆盖
        cout << "Derived speaking" << endl;
    }
};

int main() {
    Base* b = new Derived();
    b->speak();  // 输出：Derived speaking（运行时多态）
    delete b;
}
```

### 50.虚析构函数

在 C++ 中，**虚析构函数（`virtual destructor`）** 是实现**多态删除（polymorphic delete）**所必需的关键机制。当你通过基类指针删除派生类对象时，如果析构函数不是虚的，**将不会调用派生类的析构函数，可能导致资源泄漏！**

**什么时候把基类的析构函数必须实现成虚函数?**

基类的指针(引用)指向堆上new出来的派生类对象的时候，delete pb(基类的指针）它调用析构函数的时候，必须发生**动态绑定**，否则会导致派生类的析构函数无法调用；

看一个例子就懂了：

```c++
class Base {
public:
    ~Base() { std::cout << "Base destructor\n"; }
};

class Derived : public Base {
public:
    ~Derived() { std::cout << "Derived destructor\n"; }
};

int main() {
    Base* p = new Derived();
    delete p;  // ❌ 只会调用 Base 的析构函数，Derived 的不会调用
}
```

**输出：**

```c++
Base destructor
```

✅ **正确做法：**

虚析构函数就是动态绑定了

```c++
class Base {
public:
    virtual ~Base() { std::cout << "Base destructor\n"; }
};

class Derived : public Base {
public:
    ~Derived() { std::cout << "Derived destructor\n"; }
};
```

**输出：**

```C++
Derived destructor  
Base destructor
```



1、**什么时候不需要虚析构？**

- 如果**不会通过基类指针 delete 对象**，不必使用虚析构。
- 若类设计为**非继承型工具类或资源管理类（如 RAII 封装）**，也不需要。

2、**是不是虚函数调用一定就是动态绑定？**

不是的，**在类的构造函数中，调用虚函数，也是静态绑定；**

如果不是通过指针或者引用变量来调用虚函数，那就是静态绑定；

### 51.虚基类

### 52.函数调用过程参数是怎么传递的？

参数先从右到左的顺序压栈，然后压入下一行指令的地址，



### 53.为什么函数调用的参数要从右往左压栈？

因为C/C++要支持可变参函数

