练习1.1 下面是一系列表达式，对于每个表达式，解释器将输出什么结果？假定这一系列表达式是按照给出的顺序逐个求值的。

10，12，8，3，6，19，false，4，16，6，16

练习1.2 请将下面表达式变换为前缀形式：

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
