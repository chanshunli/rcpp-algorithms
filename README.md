# Rcpp Algorithms, R的快速建模 和 C++无限的资源互操作
* Compare R and C++
* Interop R and C++(使用无限的C++算法库资源,OpenCV,TensorFlow,各种机器学习和深度学习,机器视觉,语音识别算法库)
* R package develop

### cppFunction & sourceCpp
```r
(require (Rcpp))

(cppFunction ('int one() {
  return 1;
}'))

(one ()) #=> "[1] 1" 标量输出
```
* fib_cpp_1.cpp
```c++
#include <Rcpp.h>
using namespace Rcpp;

//[[Rcpp::export]]
int fib_cpp_1(int n)
{
    if(n==1||n==2) return 1;
    return fib_cpp_1(n-1)+fib_cpp_1(n-2);
}
```
```r

> require(Rcpp)
载入需要的程辑包：Rcpp
>

>  sourceCpp('fib_cpp_1.cpp')

> fib_cpp_1(50)
[1] -298632863

> fib_cpp_1(10)
[1] 55
> fib_cpp_1(20)
[1] 6765
> fib_cpp_1(30)
[1] 832040

> system.time(fib_cpp_1(50))
  用户   系统   流逝
27.053  0.356 28.620
>

```
### Matrix & Vector
##### C++ 操作 R数值向量
```r
(cppFunction ('double sumC(NumericVector x) {
  int n = x.size();
  double total = 0;
  for(int i = 0; i < n; ++i) {
    total += x[i];
  }
  return total;
}'))

(sumC (c (1, 1, 3))) #=> [1] 5
```
