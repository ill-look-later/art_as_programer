# lambda 表达式的秘密与陷阱

```c++
#include <algorithm>
#include <cmath>
void abssort(float* x, unsigned n) {
    std::sort(x, x + n,
        // Lambda expression begins
        [](float a, float b) {
            return (std::abs(a) < std::abs(b));
        } // end of lambda expression
    );

}
```

![lambda图示](/MacIOS_development/img/IC251606.jpeg)

1. Capture 子句（在 C++ 规范中也称为 lambda 引导。）
2. 参数列表（可选）。 （也称为 lambda 声明符)
3. 可变规范（可选）。
4. 异常规范（可选）。
5. 尾随返回类型（可选）。
6. “lambda 体”

Lambda 可在其主体中引入新的变量（用 C++14），它还可以访问（或“捕获”）周边范围内的变量。 Lambda 以 Capture 子句（标准语法中的 lambda 引导）开头，它指定要捕获的变量以及是通过值还是引用进行捕获。 有与号 `(&)` 前缀的变量通过引用访问，没有该前缀的变量通过值访问。空 capture 子句 `[ ]` 指示 lambda 表达式的主体不访问封闭范围中的变量。

可以使用默认捕获模式（标准语法中的 capture-default）来指示如何捕获 lambda 中引用的任何外部变量：`[&]` 表示通过引用捕获引用的所有变量，而 `[=]` 表示通过值捕获它们。 可以使用默认捕获模式，然后为特定变量显式指定相反的模式。 例如，如果 lambda 体通过引用访问外部变量 total 并通过值访问外部变量 factor，则以下 capture 子句等效：

```c++
//下面所有的写法都表示 对变量`factor`使用值捕获， 对`total`使用应用捕获
[&total, factor]
[factor, &total]
[&, factor]
[factor, &]
[=, &total]
[&total, =]
```



