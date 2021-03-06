# 第四课

如果没有某种方法可以让我们的程序作出适当的选择，加速一些冗长的进程的话，想要写出一些有用的东西将会是极其困难的。在这一节，我们将要开始条件语句和循环语句的学习。

## 条件语句：选择执行一定的代码

即使在编程之中，也不是所有的东西都是一定的。If-then 逻辑就是可以让程序处理特殊条件的方法之一，就像 “if I see a one-eyed, one-horned flying purple people eater coming my way, then I'm going the other way.” 那么让我们尽快来了解下面的内容吧！

一个计算机也可以处理同样的情况，例如，如果某个变量与 10 相等，那么就把另一个变量设置为 false。如果让我们来写这一段代码，如下所示即为所写：

<pre>
if (someVariable == 10)
{
    someOtherVariable = false;
}
</pre>

条件的声明一定要放在 if 之后的圆括号中。并且要注意不要使用单个等号来判断两个变量的值相等，只有两个等号才可以。在 C 和 C++ 中，单个等号用来把给一个变量进行赋值，两个等号才是用来比较两个值是否相等。如果你忘记了这一用法，那么编译器就会发出警告。尽管你的程序仍然可以进行编译，但是编译器会给出可能出现的编程错误的警告。在圆括号中的条件声明之后是一对大括号括起来的语句，如果条件判断是正确的，那么这里面的语句将会被执行。如果条件是不正确的，程序将会跳过大括号中的语句（换句话说，这里面的内容将不会被执行），直接执行之后的语句。当然我们也可以使用 else 语句对条件判断不正确后所要进行的操作进行指定。

<pre>
// 如果小龙喷出了火焰。
if (dragonBreathesFire)
{
    //停下来，摘取地上新鲜的蘑菇 。Yum!
    SetSpeedMPH(0);
    EquipMarshmallows(true);
}
else
{
    // 如果不是可以喷火的龙，那么就开始战斗。
    DrawSword();
    RaiseShield();
    myVelocityMPH += 4;
}
</pre>

If 声明有许多不同的使用方法。他们可以链接起来使用或者嵌套使用，如果你希望 Work Smarter, Not Harder（-a.k.a.一个懒惰的程序员），如果只有一条代码要执行，那么你可以不使用花括号将代码包括在里面：

<pre>
// 下面是if语句的简单用法 - 仅有
// 一行可以运行的语句。
if (someVariable == 10)
    someOtherVariable = false;
if (PoliceVisible())
{
    myStress++;
    // 下面是放置在一起的许多if语句。
    // 每次嵌套低优先级语句时，我们
    // 需要增加一个级别的缩进量。
    if (GetSpeed() > GetSpeedLimit())
        if (PoliceLightsFlashing())
            SetSpeedMPH(0);
        else
            SetSpeedMPH(GetSpeedLimit());
    else
        myStress--;
}
</pre>

在上面的代码中：首先，我们需要检测是否有警察出现；如果有，我们的压力值自然就会增加一些；然后检查我们的车速是否超速。如果没有，我们的压力值就会减小到这之前的程度；如果出于某些原因，车速超限了，那么就需要检查警车的警灯是否在闪，然后减速停车，接受调查。

对不同等级的嵌套使用不同的缩进是一种有目的的编码风格，这可以使你的 if-then 语句逻辑容易理解，帮助你快速判断哪一条指令属于哪一个 if 语句。在使用缩进时有两种方案可供选择：一系列的空格或者使用制表符。为了与其他章节保持一致，这里我们使用制表符，这样做的效果是相同的，但是仅需要很少的输入。再次提倡大家，work smarter，not harder。

为了给我们的行囊中添加最后一个利器，我们可以使用布尔逻辑操作符 AND、OR、或者 NOT 修改和链接条件语句，并且使用圆括号来区别优先级。对于那些不熟悉的开发者，布尔逻辑只是用于把条件判断链接起来的一种方法，例如，IF 某个条件 AND 其他条件。

<table border="1">
 <tr> <th布尔值 </th>    <th> 操作符</th>  <th> 示例 </th>                  <th> 描述 </th>                 </tr>
 <tr> <td>AND	 </td>   <td> &&	</td> <td> if(a == 1 && b == 2) </td>   <td> 如果a等于1并且b等于2 </td> </tr>
 <tr> <td>OR	 </td>   <td> ||	</td> <td> if(a == 1 || b == 2)         <td> 如果a等于1或者b等于2 </td> </tr>
 <tr> <td>NOT	 </td>   <td> !	    </td> <td> if(!(a == 1 || b == 2))</td> <td> 如果相反于a等于1并且b等于2</td> </tr>
</table> 

需要注意的是，如果您有在if语句中有多个条件语句，把每个条件放置在圆括号中将会使您的代码看起来整洁优雅，除非这些条件语句都非常的短小。

<pre>
if((a == 1 && b == 2) ||c)
	DoSomething();
</pre>
	
本示例表示的意思是“如果条件 a==1 与 b==2 为真或者 c 为非零值，那么就调用 DoSomething()”。在本示例中，整型也可以用于逻辑操作。零被视为假值，而其他的任何数则视为真值，所以在本示例中如果c是除了零以外的其他数，则条件 c 为真。

现在所有这些可能看起来马上可以用于处理很多东西，然而我们现在所学的东西if语句只是用于控制 C++ 程序运行的其他方法的基础。事实上，它还不是很复杂。

## 循环语句

循环语句是 C 和 C++ 程序的一个特性，在循环语句中，当某个条件为真时，程序将会重复的执行一系列的指令。C++ 中有几种不同的循环格式，但是在这一节，我们只探讨其中的一类：for 循环。那么接下来我们来看一段代码，然后查看其中的细节。

<pre>
#include <stdio.h>
 
int main(void)
{
      int number = 0;
      // 下面是for循环语句
      for (int i = 1; i < 10; i++)
      {
            number += i;
            printf("At step %d, the number is now %d\n",i,number);
      }
}
</pre>

当我们运行这个程序时，它会打印出如下内容：

<pre>
At step 1, the number is now 1
At step 2, the number is now 3
At step 3, the number is now 6
At step 4, the number is now 10
At step 5, the number is now 15
At step 6, the number is now 21
At step 7, the number is now 28
At step 8, the number is now 36
At step 9, the number is now 45
</pre>

我们的 for 循环有两个部分：圆括号中的控制部分和大括号中的重复指令部分。for 循环的控制部分又可分为三个部分：初始化，循环条件，以及步进值。各个部分之间由分号隔开。

初始化部分为循环初值赋予一个变量。我们可以在循环的该部分声明变量或者使用已经进行了声明的变量。通常，我们在循环的该部分声明索引变量，而且在许多程序员将会在简单循环中使用小写字符 i (index的缩写)来表示索引变量。

循环条件是一个表达式，当条件为真时，循环语句将会重复执行。在本示例中，如果 i 小于 10，那么循环将会一直重复执行。步进值是一个不断调整索引值的表达式。在多数情况下，如本示例中，我们只需要索引值每次加一即可，但是我们也可以使用任何的数学表达式－如果我们希望 i 每次自加 2，则可以将其修改为`i += 2`。与其他 C 和 C++ 运算一样，在构建 for 循环时，可以允许有很大的灵活度，但是在这里我们将会保持编程的简洁性。

## 概念应用

那么接下来我们将要把本节所学的内容应用到更加实用的地方。当您从银行取得汽车抵押贷款之后，您可能需要知道将来的付款是多少，那么接下来，我们就创建一个函数来计算付款的数额。下面是用于计算该付款数额的表达式：

这个式子显然是一个专用的函数，可能看起来很复杂。那么我们就把这个可能有点混乱的数学式子解释为我们可以容易理解的内容。

<pre>
#include <math.h>
 
float Payment(float principal, float rate, int months)
{
      float top, bottom;
 
      // 下面这一行用于计算右边表达式的顶端部分。
      top = principal * (rate / 12.0);
      // 而下面一行则用于计算右边表达式的底端部分。
      bottom = 1 - pow(1 + (rate / 12.0),-months);
 
      return (top / bottom);
}
</pre>

把一个表达式表示为函数，就是要把它分为几个可以控制的部分。我们分别计算了两部分的结果，然后分别把结果赋值给两个不同的变量。这样做可以使我们的代码易于阅读和调试。

需要注意的是，我们使用了 12.0 而不是 12 来强制编译器将 12 转换为浮点型数据而不是整型。在处理整型的运算时，编译器将会舍弃小数点后的结果，例如，10/4 的结果为 2，而 10.0/4.0 的结果则为 2.5。在这种情况下，如果我们希望避免四舍五入的错误，我们必须使用 12.0 而不是 12。

当然我们也可以在一行语句中完成所有的计算，但是它将非常难懂，而且调试时很让人头疼。如下所示：

`return (principal * rate / 12.0) / ( 1 - pow(1 + (rate / 12.0),-months) );`

这很让人讨厌。但是一些资深的程序员可能推崇这种做法，因为代码将会非常短小紧凑；但是易于阅读，容易维护的代码将更加重要。既然我们已经可以让这个函数来计算付款数额，那么我们可以使用它来计算持续1至5年的汽车贷款月费用。

<pre>
#include <stdio.h>
#include <math.h>
 
float Payment(float principal, float rate, int months)
{
      float top, bottom;
      top = principal * (rate / 12.0);
      bottom = 1 - pow(1 + (rate / 12.0),-months);
      return (top / bottom);
}
 
int main(void)
{
      float principal, rate;
      int months;
      principal = 10000.0;
      rate = .05;
 
      for (int months = 12; months <= 60; months += 12)
      {
            // 由于空格对编译器没有影响，我们可以将长的代码行
            // 分割成短小的代码行，从而提高代码的可读性。
            printf("The monthly payment for a %d month, $%f car loan "
                  "at %f%% is $%f\n", months, principal, rate * 100,
                  Payment(principal,rate,months));
      }
 
}
</pre>

它可以很好的执行！运行该函数时，它将会显示一年的抵押贷款是很昂贵的，而4至5年的贷款则可以更容易的接受。

## 深入练习

尝试使用下面的数字进行练习，查看结果如何，例如，如果循环条件从 60 修改为 72，或者原始本金为 ￥20000 而不是 ￥10000。