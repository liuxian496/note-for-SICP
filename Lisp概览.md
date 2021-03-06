
# Lisp概览

## 关键词Lec1A-part1

关键词 | 解释
---|---
process | 过程
procedure | 程序
techniques for controling complexity | 控制复杂性的技术
black-box abstraction | 黑盒抽象
conventional interfaces | 约定接口
Metalinguistic Abstraction | 元语言抽象


## 笔记Lec1A-part1

* 通过可重复性的有限步骤，逐渐接近预期结果（在一定精度上）的行为，称为process【过程】
 
* 计算机中，存在着完成一些操作的指令，被称为procedure【程序】的规则模式，指导着这类指令的运行。使用procedure控制这些指令，完成预期操作的行为，称为process【过程】。

* 编程语言是人在编写程序时，使用的魔法，更确切的说是一种基于人类语言的形式系统。

* 复杂的系统，背后会有大量的程序代码，大到不可能直观的分析。使用技术控制这种复杂性，就是计算机技术的关键。

* 面对一个复杂的现实问题时，人们在烦恼和不断尝试中，会试图寻找或创造特定的规则来解决它，或者降低它的复杂度。这个过程往往伴随生产力的提高。

* 与现实世界不同，计算机科学处理的组件比现实世界中的更加理想。在对程序和数据了如指掌的情况下(没有bug)，不需要去关心公差和系统噪声。在构建大型软件时，可以随心所欲使用这些组件。这是与其他现实世界的工程，比如建筑的根本区别。后者受限于物理系统，近似值，材料强度等，而计算机科学只受限于我们的大脑。

* 黑盒抽象,即将一些东西组合并封装起来。

* 黑盒抽象基本规则：通关把处理过程放到盒子内隐藏其实现细节，以便用已有的盒子构建更大的盒子。

* 通过对现实世界的复杂系统建模来构建大型程序时,有两种非常重要的方法：
    
    1. object-oriented programing【面向对象编程】：使用这个方法构建的系统，类似一个社区，社区中的各个部分通过相互传递消息联系起来。
    
    2. operations on aggregates【聚焦的操作】称作“流”：类似于电力工程师构造大型电器系统

* 可以通过定义一种新“语言”来控制系统复杂度。设计新语言的意图是为了强调系统的某个方面。一方面隐藏了部分细节,但另一方面强调一些其他细节。

## 关键词Lec1A-part2

关键词 | 解释
---|---
primitive elements | (构成语言的)基本元素
means of combination | 将基本元素组合起来的方法
means of abstraction | 抽象的方法，指的是如何利用基本元素和它们的组合，并把它们封装成黑盒的方法。

## 笔记Lec1A-part2

* 计算机语言三大特性：
    1. primitive elements
    2. means of combination
    3. means of abstraction

* combination【组合式】：一个组合式，由operator【运算符】和应用该运算符的operands【运算对象】组成。

* operand【运算对象】也可以是组合式。

* 组合式是组合的基本需求。

* 前缀表示法：（+ 3 (* 5 6) 5），操作符再操作数的左端。

* syntactic sugar【语法糖】的意思就是，这种形式输入更方便一些。

* (DEFINE (SQUARE x) (* x x))
  (SQUARE 100)
  (SQUARE (+ 5 7))
  (+ (SQUARE 5) 7)

## 关键词Lec1A-part3

关键词 | 解释
---|---
recursive definition【递归定义】 | 在不增加系统负担下，在达到条件（递归出口）时，完成无限次计算
lexical scoping【词法作用域】 | 规定了变量的引用范围
block structure【块结构】 | 把东西打包到定义内部的一种方法，避免过多的定义污染，使使用者不便。

## 笔记Lec1A-part3

* 递归计算平方根

```
(define (sqrt-iter guess x)
  (if (good-enough? guess x)
      //递归出口
      guess
      (sqrt-iter (improve guess x) x)))

(define (improve guess x)
  (average guess (/ x guess)))

(define (average x y)
  (/ (+ x y) 2))

(define (good-enough? guess x)
  //递归出口
  (< (abs (- (square guess) x)) 0.001))

(define (sqrt x)
  (sqrt-iter 1.0 x))

//计算9的平方根
(sqrt 9)

//计算137的平方根
(sqrt (+ 100 37))


```

![Lec1A-p3-1.png](https://github.com/liuxian496/note-for-SICP/blob/master/img/Lec1A-p3-1.png)

对使用者来说，重要的是sqrt，这种分开定义的方式容易使使用者混淆。快结构是解决打包命名问题的最简单方法。这样只有sqrt提供给使用者。改进后的算法：

```
(define (sqrt x)
  (define (good-enough? guess x)
    (< (abs (- (square guess) x)) 0.001))
  (define (improve guess x) (average guess (/ x guess)))
  (define (sqrt-iter guess x)
          (if (good-enough? guess x)
              guess
              (sqrt-iter (improve guess x) x)))
(sqrt-iter 1.0 x))

```

使用快结构时，由于在最外层传入了x，内部的定义都可以使用，x可以省略（词法作用域）。代码如下：

```
(define (sqrt x)
  (define (good-enough? guess)
    //递归出口
    (< (abs (- (square guess) x)) 0.001))
  (define (improve guess)
    (average guess (/ x guess)))
  (define (sqrt-iter guess)
    (if (good-enough? guess)
        //递归出口
        guess
        (sqrt-iter (improve guess))))
(sqrt-iter 1.0))

```

* (DEFINE A (* 5 5))
  
  //D相当于其他语言中的函数
  (DEFINE (D) (* 5 5))

  输入A，值是25

  输入D，值是 compound procedure D

  输入(D)，值是25
  输入(A)，值是error

