
# Rcpp Algorithms, R的快速建模 和 C++无限的资源互操作
* Compare R and C++
* Interop R and C++(使用无限的C++算法库资源,OpenCV,TensorFlow,各种机器学习和深度学习,机器视觉,语音识别算法库)
* R package develop



- [Rcpp Algorithms, R的快速建模 和 C++无限的资源互操作](#rcpp-algorithms-r%E7%9A%84%E5%BF%AB%E9%80%9F%E5%BB%BA%E6%A8%A1-%E5%92%8C-c%E6%97%A0%E9%99%90%E7%9A%84%E8%B5%84%E6%BA%90%E4%BA%92%E6%93%8D%E4%BD%9C)
    - [cppFunction & sourceCpp](#cppfunction--sourcecpp)
    - [Matrix & Vector](#matrix--vector)
        - [C++ 操作 R数值向量](#c-%E6%93%8D%E4%BD%9C-r%E6%95%B0%E5%80%BC%E5%90%91%E9%87%8F)
        - [C++ 向量输出`NumericVector outVector(ys.size())`创建一个长度为n的数值向量](#c-%E5%90%91%E9%87%8F%E8%BE%93%E5%87%BAnumericvector-outvectoryssize%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AA%E9%95%BF%E5%BA%A6%E4%B8%BAn%E7%9A%84%E6%95%B0%E5%80%BC%E5%90%91%E9%87%8F)
        - [c++操作矩阵, 一层层for操作(就像生成学号一样)](#c%E6%93%8D%E4%BD%9C%E7%9F%A9%E9%98%B5-%E4%B8%80%E5%B1%82%E5%B1%82for%E6%93%8D%E4%BD%9C%E5%B0%B1%E5%83%8F%E7%94%9F%E6%88%90%E5%AD%A6%E5%8F%B7%E4%B8%80%E6%A0%B7)
        - [c++ 矩阵输出](#c-%E7%9F%A9%E9%98%B5%E8%BE%93%E5%87%BA)




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
##### C++ 向量输出`NumericVector outVector(ys.size())`创建一个长度为n的数值向量
```r
(cppFunction ('NumericVector pdistC(double x, NumericVector ys) {
  int n = ys.size();
  NumericVector out(n);

  for(int i = 0; i < n; ++i) {
    out[i] = sqrt(pow(ys[i] - x, 2.0));
  }
  return out;
}'))

(pdistC (-1, (c (2, 2)))) #=> [1] 3 3
```
##### c++操作矩阵, 一层层for操作(就像生成学号一样)
```r
# x(i, j) c++中用()对矩阵进行子集选取
(cppFunction ('NumericVector rowSumsC(NumericMatrix x) {
  int nrow = x.nrow(), ncol = x.ncol();
  NumericVector out(nrow);

  for (int i = 0; i < nrow; i++) {
    double total = 0;
    for (int j = 0; j < ncol; j++) {
      total += x(i, j);
    }
    out[i] = total;
  }
  return out;
}'))

(rowSumsC (matrix ((sample (100)), 10))) #=> [1] 489 515 476 415 547 551 357 663 533 504
```
##### c++ 矩阵输出
```r
(cppFunction ('NumericMatrix zeroMat(int x, int y) {
  NumericMatrix outMat(x, y);
  return outMat;
}'))

(zeroMat (3, 2))
#=>
      [,1] [,2]
 [1,]    0    0
 [2,]    0    0
 [3,]    0    0
```
### 将C++函数和类导出到R中 -> Rcpp Modules
```r

```
