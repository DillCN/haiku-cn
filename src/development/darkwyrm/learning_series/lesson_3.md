#  第三课
在前两节里，我们学习了一些编程的基础知识，例如如何编写一个函数，如何为我们的代码找出bug。那么在这一节里我们将学习不同的数据类型以及它们的存储和传递方式。

由于直接进行数据的计算如 “return 1+1” 将会显得非常麻烦，我们在编程中引入了变量的概念。一个变量就是一个信息的储存容器，就如我们真实世界中的容器一样，它们也有不同的类型，大小以及用途。

创建一个变量其实就是声明它的存在，如下：

    #include <stdio.h>
    int main(void)
    {
        // 声明一个整型变量 a
        int a;
        // 同时声明两个整型变量: b and c
        int b, c;
        a = 1;
        b = 2;
        c = 3;
        //在屏幕上显示变量的值
        printf("a is %d, b is %d, and c is %d.\n",a,b,c);
        return a + b + c; 
    }
    
变量可以一次只声明一个，也可以一次进行多个声明。除了在函数中声明变量之外，由于许多函数需要接收外部数据进行处理，我们需要为这些数据声明一些变量，这些变量称为 parameters 或者 arguments（即参数）。

函数参数在该函数的声明和定义时都需要声明，它们存在于函数后面的圆括号中，被一串逗号分割开来。如果一个函数不接收任何的变量，那么就在它的圆括号中使用 “void” 来声明。

    // 声明一个有两个参数的函数.
    int SomeFunction(int someNumber, int anotherNumber);
    // 定义一个有两个参数的函数.
    int MultiplyNumbers(int value, int secondValue)
    {
        return value * secondValue;
    }
    // 一个没有参数的函数. 这个函数一定得相当的安静吧.
    int main(void)
    {
        return MultiplyNumbers(2,3);
    }
    
在调用一个有参数的函数时，例如上面例子中的 MultiolyNumbers()，我们不需要列出每一个变量类型，因为编译器会自动跟踪变量类型，并且在出错的时候给出警告。

涉及到变量类型，在 C++ 中不只有整型这么一种。每一种数据类型在内存中都占用的空间大小各不相同，也就是所需的字节数不同。由于每一种数据类型所占用的字节数决定了它可以存储信息的大小，记住这些类型以及它们所占用的字节数是非常重要的。当我们在比较 char 类型和 short 类型的数据范围时，这种差异将会极为明显。由于数据类型在不同计算机平台之间的差异，我们有必要介绍一下在 32 位处理器上 Haiku 的不同数据类型。

<table border="1">
    <tr> <th>数据类型</th>        <th>字节数</th> <th>数值范围</th>       <th>描述</th> </tr>
    <tr> <td> Char </td>          <td> 1 </td>    <td> -128～127 </td>    <td> 字符，一个char变量只能够存储一个字符</td> </tr>
    <tr> <td> Unsigned char </td> <td> 1 </td>    <td> 0～255 </td>        <td> 同上</td> </tr>
    <tr> <td> Short </td>         <td> 2 </td>    <td> -32768～32767 </td> <td> 整数 </td> </tr>
    <tr> <td> Unsigned short </td> <td> 2 </td>   <td> 0～65535 </td>      <td> 整数 </td> </tr>
    <tr> <td> Int </td>            <td> 4 </td>   <td> -2147483648～2147483647 </td> <td> 整数 </td> </tr>
    <tr> <td> Unsigned int </td>   <td> 4 </td>   <td> 0～4294967295 </td>     <td> 整数 </td> </tr>
    <tr> <td> Long </td>           <td> 4 </td>   <td> -2147483648～2147483647 </td> <td> 整数 </td> </tr>
    <tr> <td> Long long </td>      <td> 8 </td>   <td> -9223372036854775808～9223372036854775807 </td> <td> 整数 </td> </tr>
	<tr> <td> Unsigned long long </td> <td> 8 </td><td> 0～18446744073709551615 </td> <td> 整数 </td> </tr>
	<tr> <td> Float </td>          <td> 4 </td><td> 3.4E+/-38(7 digits) </td> <td> 浮点数 </td> </tr>
    <tr> <td> Double </td>         <td> 8 </td><td> 1.7E+/-308(15 digits) </td> <td> 浮点数 </td> </tr>
    <tr> <td> Long double </td>    <td> 12 </td><td>  </td> <td> 浮点数 </td> </tr>
    <tr> <td> Bool </td>    <td> 1 </td><td>  TRUE or FALSE</td>  <td> 真值或假 </td> </tr>
    <tr> <td> Wchar_t </td>    <td> 2或4 </td><td>  As either short or int</td>  <td> 可以保存整数或者字符 </td></tr>   
</table>

## 使用 printf() 函数

是否还记得之前 `printf()` 函数古怪的使用方法么？那么接下来就让我们揭开它让人迷惑面纱。

<pre>
#include <stdio.h>
int main(void)
{
    int a;
    int b, c;
    a = 1; 
    b = 2;
    c = 3;
    printf("a is %d, b is %d, and c is %d.\n",a,b,c);
    return a + b + c;
}
</pre>

Printf 是仅有的几个所接收参数数目可变的函数之一。它的第一个参数总是一个字符串，在这一字符串之后可以有附加的参数，这取决于字符串中格式控制符的数量。在上面的例子中，字符串中有三个 “%d” 格式控制符，分别用来格式化输出 a,b,和 c。有三个格式控制符就有三个附加的参数。下面将要介绍的是以后编程将要用到的格式控制字符。需要注意的是，这里并没有包涵所有有关 printf 函数的格式控制符，但是这些对于我们来说已经足够了。

<table border="1">
    <tr> <th>格式控制符</th>     <th>数据类型</th>       <th>输出示例</th>          </tr>
    <tr> <td> %c    </td>        <td> 字符型 </td>       <td> A </td>               </tr>
    <tr> <td> %d,%i </td>        <td> 有符号整型 </td>   <td> 234 </td>             </tr>
    <tr> <td> %e,%E </td>        <td> 科学计数型 </td>   <td> 1.7e+5,1.7E+5 </td>   </tr>
    <tr> <td> %f    </td>        <td> 浮点型 </td>       <td> 3.14 </td>            </tr>
    <tr> <td> %g    </td>        <td> 双精度浮点型 </td> <td> 3.14 </td>            </tr>
    <tr> <td> %o    </td>        <td> 八进制符号整数 </td> <td> 711 </td>           </tr>
    <tr> <td> %u    </td>        <td> 无符号整型 </td>     <td> 255 </td>           </tr>
    <tr> <td> %x,%X </td>        <td> 十六进制整数 </td>   <td> 0xff,0xFF </td>     </tr>
	<tr> <td> %%    </td>        <td> 百分号 </td>          <td> % </td>            </tr>  
</table>

## 运算符

运算符给我们提供了一种可以不需要函数的调用就可以实现变量和数字的计算的方法。“+”，“-”和“*”都是运算符。C++ 提供了除此之外更多的运算符，下面表格中的就是我们常用到的一些运算符。

<table border="1">
 <tr> <th>运算符 </th>    <th> 执行操作	    </th><th>   操作描述 </th> </tr>
 <tr> <td>a+b	 </td>   <td> 加法	         </td> <td> 将a,b相加</td>    </tr>
 <tr> <td>a-b	 </td>   <td> 减法	         </td> <td> 将a减去b</td>    </tr>
 <tr> <td>a*b	 </td>   <td> 乘法	         </td> <td> 将a乘上b</td>    </tr>
 <tr> <td>a/b	 </td>   <td> 除法	         </td> <td> 将a除去b</td>    </tr>
 <tr> <td>a%b	 </td>   <td> 取余	         </td> <td> 对a除去b取余</td> </tr>
 <tr> <td>a=b	 </td>   <td> 赋值	         </td> <td> 将b的值赋给a</td> </tr>
 <tr> <td>++a	 </td>   <td> 前置自加       </td> <td> 执行其他运算之前将a自加1</td>  </tr>
 <tr> <td>a++	 </td>   <td> 后置自加       </td> <td> 执行其他运算之后将a自加1</td>  </tr>
 <tr> <td>--a	 </td>   <td> 前置自减       </td> <td> 执行其他运算之前将a减去1</td>  </tr>
 <tr> <td>a--	 </td>   <td> 后置自减       </td> <td> 执行其他运算之后将a减去1</td>  </tr>
 <tr> <td>a+=b	 </td>   <td> 复合加法运算符</td> <td>a=a+b的简写形式</td>    </tr>
 <tr> <td>a-=b	 </td>   <td> 复合减法运算符</td> <td>a=a-b的简写形式</td>    </tr>
 <tr> <td> a*=b </td>    <td> 复合乘法运算符</td> <td>a=a*b的简写形式</td>    </tr>
 <tr> <td> a/=b </td>    <td> 复合除法运算符</td> <td>a=a/b的简写形式</td>    </tr>
 <tr> <td> a%=b	 </td>   <td> 复合取余运算符</td> <td>a=a%b的简写形式</td>    </tr>
</table> 

“—”和“++”两个运算符在开始使用之前还需要对它们的使用方法进行一些介绍。我们以下面的代码为例来进行介绍。
<pre>
#include <stdio.h>
int main(void)
{
    int a = 1;
    int b = 2;
    // 由于我们首先执行的是 a+b，之后再进行a=a+1.
    // 所以打印出来的结果是 3
    printf("a++ + b = %d\n",a++ + b);
    // 因为我们在前一步执行了a=a+1,所以这里的结果将是4.
    printf("a + b = %d\n",a + b);
    // 由于编译器在计算求和之前a自加1，所以这里的结果是5.
    // a + b
    printf("++a + b = %d\n",++a + b);
    return 0;
}
</pre>

在这一节里我们涉及到了很多的内容但是并没有进行具体的应用，那么接下来我们开始对如何实际应用上面的内容进行讲解。

<pre>
#include <stdio.h>
// math.h 给我们提供了很多有关数学公式的函数. 
// 在头文件里包含 math.h，那么接下来我们就可以直接使用 sqrt()函数,这一函数
// 可以用来求开方.
#include <math.h>
double hypotenuse(int a, int b)
{
    return sqrt((a*a) + (b*b));
}
int main(void)
{
    int a = 3;
    int b = 4;
    printf("For the triangle with legs %d and %d, the hypotenuse will be %g\n", a, b, hypotenuse(a,b));
    return 0;
}
</pre>

由于在上面的计算中，我们要求一定精度的数值，这个数超出了整数的范围，所以 `Hypotenuse()` 函数返回了一个双精度的数，而这也是 `sqrt()` 函数的返回值类型。

## 找错

* 找错 #1
    * 源码
	    <pre>
        int sum(int first, int second, int third)
        {
            return first + second + third;
        }
        int main(void)
        {
            int a = 3;
            int b = 4;
            printf("The sum is %d\n", sum(a,b,c));
            return 0;
        }
        </pre>
    * 错误
		<pre>
        foo.cpp: In function ‘int main()’:
        foo.cpp:14: error: ‘c’ was not declared in this scope
		</pre>
 
* 找错 #2
    * 源码
	    <pre>
        #include <stdio.h>
        double distance(int x1, int y1, int x2, int y2)
        {
            int deltax = x2 - x1;
            int deltay = y2 - y1;
            return sqrt((deltax * deltax) + (deltay * deltay));
        }
        int main(void)
        {
            int x1, y1, x2, y2;
            x1 = 3;
            y1 = 3;
            x2 = 8;
            y2 = 3;
            printf("The distance between (%d, %d) and (%d, %d) is %g\n", x1,y1, x2,y2, distance(x1,y1,x2,y2));
            return 0;
        }
        </pre>
    * 错误
	    <pre>
        foo.cpp: In function ‘double distance(int, int, int, int)’:
        foo.cpp:8: error: ‘sqrt’ was not declared in this scope
		</pre>
 
## 作业

使用方程 `interest=Principal*rate*time`，计算当酬金为 20000 美元而以每月 5% 的进度需要 24 个月来完成一项工作的兴趣值，并且需要使用一个单独的函数来完成这项工作。