练习1.1 下面是一系列表达式，对于每个表达式，解释器将输出什么结果？假定这一系列表达式是按照给出的顺序逐个求值的。  
10 [10]   
(+ 5 3 4) [12]   
(- 9 1) [8]    
(/ 6 2) [3]    
(+ (* 2 4) (- 4 6)) [6]     
(define a 3)   
(define b (+ a 1))   
(+ a b (* a b)) [19]    
(= a b) [false]   
(if (and (> b a) (< b (* a b)))
    b
    a) [4]       
(cond ((= a 4) 6)
      ((= b 4) (+ 6 7 a))
      (else 25)) [16]       
(+ 2 (if (> b a) b a)) [6]     
(* (cond ((> a b) a)
         ((< a b) b)
         (else -1))
    (+ a 1)) [16]    
10，12，8，3，6，19，false，4，16，6，16  

练习1.2 请将下面表达式变换为前缀形式：  

(5 + 4 + (2 - (3 - (6 + 4 / 5)))) / (3 * (6 - 2) * (2 - 7))

(/ (+ 5 4 (- 2 (- 3 (+ 6 (/ 4 5))))) (* 3 (- 6 2) (- 2 7)))  

练习1.3 请定义一个过程，它以三个数为参数，返回其中较大的两个数之和。  

(define (plus-max-two a b c)  
  (cond ((and (> b a) (> c a)) (+ b c))  
        ((and (> a b) (> c b)) (+ a c))  
        (else (+ a b))))  

练习1.4 请仔细考察上面给出的允许运算符为复合表达式的组合式的求值模型，根据对这一模型的认识描述下面过程的行为：  
(define (a-plus-abs-b a b)  
  ((if (> b 0) + -) a b))  

该过程是求a与b的绝对值之和。条件判断选择的是一个运算符，对(> b 0)求值，条件为真则选择 + 运算符，否则选择 - 运算符。  

练习1.5 Ben Bitdiddle发明了一种检测方法，能够确定解释器究竟采用哪种序求值，是采用应用序，还是选择正则序。他定义了下面两个过程：  
(define (p) (p))  
(define (test x y)  
  (if (= x 0)  
      0    
      y))   
而后他求值下面的表达式：   
(test 0 (p))  
如果某个解释器采用的是应用序求值，Ben会看到什么样的情况？如果解释器采用正则序求值，他又会看到什么情况？请对你的回答做出解释。     
（无论采用正则序或者应用序，假定特殊形式if的求值规则总是一样的。其中的谓词部分先行求值，根据其结果确定随后求值的子表达式部分。）   

(define (p) (p))语法错误。  

练习1.6 Alyssa P. Hacker看不出为什么需要将if提供为一种特殊形式，她问：“为什么我不能直接通过cond将它定义为一个常规过程呢？”   
Alyssa的朋友Eva Lu Ator断言确实可以这样做，并定义了if的一个版本：   
(define (new-if predicate then-clause else-clause)   
  (cond (predicate then-clause)   
        (else else-clause)))   
Eva给Alyssa演示了她的程序：   
(new-if (= 2 3) 0 5)   
5   
(new-if (= 1 1) 0 5)   
0   
她很高兴地用自己的new-if重写了求平方根的程序：  
(define (sqrt-iter guess x)  
  (new-if (good-enough? guess x)  
          guess  
          (sqrt-iter (improve guess x) x)))  
当Alyssa试着用这个过程去计算平方根时会发生什么事情呢？请给出解释。  


练习1.7 对于确定很小的数的平方根而言，在计算平方根中使用的检测good-enough?是很不好的。还有，在现实的计算机中总是以一定的有限精度进行的。   
这也会使我们的检测不适合非常大的数的计算。请解释上述的论断，用例子说明对很小和很大的数，这种检测都可能失败。实现good-enough?的另一种策略   
是监视猜测值在从一次迭代到下一次的变化情况，当改变值相对于猜测值的比率很小时就结束。请设计一个采用这种终止测试方式的平方根过程。对于很大   
和很小的数，这一方式都能工作吗？   

书中原代码：  
(define (sqrt-iter guess x)   
  (if (good-enough? guess x)   
    guess   
    (sqrt-iter (improve guess x) x)))   

(define (improve guess x)   
  (average guess (/ x guess)))   

(define (average x y)   
  (/ (+ x y) 2))   

(define (good-enough? guess x)   
  (< (abs (- (square guess) x)) 0.001))   

(define (square x) (* x x))     

(define (mysqrt x)   
  (sqrt-iter 1.0 x))   

1.7答案：
(define (sqrt-iter lastguess guess x)   
  (if (good-enough? lastguess guess)   
    guess   
    (sqrt-iter guess (improve guess x) x)))   

(define (improve guess x)   
  (average guess (/ x guess)))   

(define (average x y)   
  (/ (+ x y) 2))   

(define (good-enough? lastguess guess)   
  (< (abs (- lastguess guess)) 0.001))     

(define (mysqrt x)   
  (sqrt-iter x 1.0 x))   

练习1.8 求立方根的牛顿法基于如下事实，如果y是x的立方根的一个近似值，那么下式将给出一个更好的近似值：  
(x / y ^ 2 + 2 * y) / 3    
请利用这一公式实现一个类似平方根过程的求立方根的过程。（在1.3.4节里，我们将看到如何实现一般性的牛顿法，作为这些求平方根和立方根过程的抽象。）  

(define (cbrt-iter left right x)
  (define middle (/ (+ left right) 2))
  (if (good-enough? middle x)   
    guess   
    (cond ())))   

(define (improve x y)
    (/ (+ x y) 2)    

(define (good-enough? guess)   
  (< (abs (- (close-stere guess) x)) 0.1))   

(define (close-stere x) (* x x x))   

(define (mycbrt x)   
  (cbrt-iter 1.0 x))   
