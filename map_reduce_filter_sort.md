# map_reduce_filter_sort

###map
map 就是将可映射的每个对象进行同一个函数的操作

```js
var arr = [0,1,2,3,4,5,6,7];
function add(x,y) {
  return x+y;
}
arr.map(add); // === 0+1+2+3+4+5+6+7
```
###Reduce
reduce 操作就是将一个可递归，可映射对象重复的利用某一个函数进行归并。函数的参数是上一次归并结果