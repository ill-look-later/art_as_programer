＃ C++ 的委托构造函数

虽然一直在使用C++开发， 但是有个问题一直没有在意， 就是C++不同的编译器和版本不一定支持`委托构造函数（Delegating constructors）`， 如果说正式的支持委托构造函数， 是在C++11标准中正式确定的；我们先来看看在工作中遇到的一个问题， 其中一些细微的差别，导致的运行结果非常不一样：

Case 1:
---

```c++
#include "stdio.h"

class CDelegate {
public:
	CDelegate() {
		printf("%s\n", "CDelegate() be called");
		CDelegate(0); //@ 这里我是在构造函数中调用的另外一个构造函数！！
	}
	CDelegate(int value) {
		printf("%s\n", "CDelegate(int value) be called");
		value_ = value;
	}
	~CDelegate() {
		printf("%s\n", "CDelegate destroyed");
	};
	int get_vaule() {
		return value_;
	}
private:
	int value_;
};

int main(int argc, char const *argv[])
{
	/* code */
	CDelegate* delegate = new CDelegate();
	printf("value_: %d\n", delegate->get_vaule());
	//delete delegate;
	return 0;
}
```

- 上面的例子中,我是在无参数的构造函数的block中调用了 带参数的构造函数；
- 是通过无参数的构造函数创建的`CDelegate`对象
- 为了排除干扰，main函数中没有去调用`delete delegate`对象

Case 1 输出
---

下面是运行结果：
```shell
CDelegate() be called
CDelegate(int value) be called
CDelegate destroyed
value_: 0
```

Case 2
---

```c++
#include "stdio.h"

class CDelegate {
public:
	CDelegate(): CDelegate(0) { //这里是在初始化器中！！！！！
		printf("%s\n", "CDelegate() be called");
	}
	CDelegate(int value) {
		printf("%s\n", "CDelegate(int value) be called");
		value_ = value;
	}
	~CDelegate() {
		printf("%s\n", "CDelegate destroyed");
	};
	int get_vaule() {
		return value_;
	}
private:
	int value_;
};

int main(int argc, char const *argv[])
{
	/* code */
	CDelegate* delegate = new CDelegate();
	printf("value_: %d\n", delegate->get_vaule());
	//delete delegate;
	return 0;
}
```
下面我们看看输出：如果我使用`g++ -std=gnu++11 main.cc` 或 `g++ -std=c++11 main.cc` 则不会出现警告⚠️；我使用的g++版本是ubuntu 16.04中自带的版本g++ 5.4
Case 2 输出:
---
```shell
huan@Macmini:~/Desktop$ g++ main.cc
main.cc:5:26: warning: delegating constructors only available with -std=c++11 or -std=gnu++11
  CDelegate(): CDelegate(0) {
                          ^
huan@Macmini:~/Desktop$ ./a.out 
CDelegate(int value) be called
CDelegate() be called
value_: 0
```

为了更清楚的看看到底发生了什么， 我对case 1改造一下， 以便我们看清楚发生了什么：
case 3
---

```c++
#include "stdio.h"

class CDelegate {
public:
	CDelegate() {
		printf("%s; this object is:%p\n", "CDelegate() be called", this);
		CDelegate(0);
	}
	CDelegate(int value) {
		printf("%s, this object is:%p\n", "CDelegate(int value) be called", this);
		value_ = value;
	}
	~CDelegate() {
		printf("obj:%p going to die, %s\n", this, "CDelegate destroyed");
	};

	int get_vaule() {
		return value_;
	}

private:
	int value_;
};


int main(int argc, char const *argv[])
{
	/* code */
	CDelegate* delegate = new CDelegate();
	printf("using obj:%p get value_: %d\n", delegate, delegate->get_vaule());
	//delete delegate;
	return 0;
}
```
Case 3 输出：
---

```shell
CDelegate() be called; this object is:0x13cfc20
CDelegate(int value) be called, this object is:0x7ffc4b06b4e0
obj:0x7ffc4b06b4e0 going to die, CDelegate destroyed
using obj:0x13cfc20 get value_: 0

```

分析与结论:
---
从case 1的输出可以看到， 如果我们在一个构造函数的函数体中去调用另一个构造函数， 实际上会发生另外一次构造函数和析构函数， 也就是说我们在构造函数CDelegate() 中通过`CDelegate(int value)`构造了一个新的CDelegate对象，而这个对象作为一个临时变量