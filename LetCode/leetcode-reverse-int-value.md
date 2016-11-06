# 反序整型数

```cpp

#include <limits>

class Solution { //6ms
public:
    int reverse(int x) {
        //bool is_negative = false;
        
        int max = 2147483647;
        int min = -2147483648;
        //std::cout << "max" << std::numeric_limits<int>::max();
        //std::cout << "min" << std::numeric_limits<int>::min();
        //if (x < 0) {
            //is_negative = true;
            //x = -x;
        //}
        long res = 0;
        do {
            res = 10*res+(x%10);
            x = x/10;
        } while(x);
        
        if (res > max || res < min)
            return 0;
        return (int)res;
    }
};
```