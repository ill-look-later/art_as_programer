# c++ access class private variable

> 看下面这段代码：
```cpp
#include <iostream>
using namespace std;
class test{
      private:
            int a;
            int b;
      public:
            test(int a = 1, int b = 2){
                  this->a = a;
                  this->b = b;
            }
            int re(test ccc){
                  a = ccc.a + 444;
                  b = ccc.b + 444;
            }
};
```