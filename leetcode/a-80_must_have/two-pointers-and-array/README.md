# Two Pointers & Array

![](<../../../.gitbook/assets/Screen Shot 2021-12-31 at 2.03.19 PM.png>)

同向：

【0，i）的数据代表处理好的，【i，j）中的数据是处理过但是不需要的数据， 【j，array.length）区间的数据位接下来待处理的数据 - i 是下一个可以被填充的数据

使用本方法处理过的数据，保存下来的数据相对位置是保持一致的 （重要，要问）

1. initialize two pointers i and j, usuallly both equal to 0
2. while j < array.length:
   1. if we need array\[j], then we keep it by assiging array\[i] = array\[j], and move i forward, make it ready at the next position
   2. otherwise skip it. we donot need to move i since its spot is not fulfilled

反向

处理过的数据不会保持相对位置

1. initialize two pointers i = 0, j = array.length - 1
2. while i <= j:
   1. deciede what you should do based on  the value of array\[i] and array\[j]
   2. move at least one pointer forward in its direction
