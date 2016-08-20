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
下面我们看看输出：
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
