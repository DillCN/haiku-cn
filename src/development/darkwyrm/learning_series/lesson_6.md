# 第六课

在本节，我们主要来扩展讲解我们已经了解的内容。在第四课中，我们了解了在 if 语句中，只有满足特定的条件，才可以运行相关的代码，并且我们也学习了如何使用 for 循环重复执行一系列的指令。那么在本节中，我们将会进一步介绍一些工具。

## 更多的循环

尽管它们可能是最常用的，但是除了 for 循环，还有许多其他类型的循环。事实上，还有其他两个非常相近的循环：while 循环，和 do-while 循环。

而且循环比较简单，仅需要一个循环条件，循环中的代码将会重复运行，直到循环条件为假。那么我们就寻找一种方法，利用 while 循环来完成我们第五课中的项目。
    
    #include <stdio.h> 
     
    int main(void) 
    { 
            char inString[1024]; 
            printf("Type some text and hit Enter:\n"); 
            gets(inString); 
     
            int i = 0; 
            while (inString[i]) 
                     printf("String[%d]: %c\n",i,inString[i++]); 
     
            return 0; 
    }

在上面的示例中，我们甚至采取了一个捷径，没有使用花括号 - 当消息打印之后，使用自增操作符实现索引变量 i 的自加。循环的条件语句检查 `inString[i]` 是否为无效字符。整型可以用于真/假测试的布尔逻辑运算，而零被视为假，所有其他的值则被视为真。因此循环将会不断执行，直到索引指向的值为字符串结束标志 0。看起来非常的诱人，是么？

除了上述的 while 语句以及跟随的条件语句，循环其他部分和 for 循环相似；因此，我们可以简单的使用花括号中的一组语句来替换我们的 `printf()` 语句。使用这种循环的一个警告：如果您对确保循环条件最终为假不关心，那么很容易就会导致无限循环。在这个实例中，如果 `inString[i++]` 仅仅是 `inString[i]`，那么我们将会创建一个无限循环。

Do-while 循环不像其他循环一样很常用，但是它偶尔也非常有用。它首先运行循环语句，然后检查循环条件再决定是否继续执行循环语句。下面是一个使用 do-while 循环语句的示例：
    
    #include <stdio.h> 
     
    int main(void) 
    { 
        char inString[1024]; 
        printf("Type some text and hit Enter:\n"); 
        gets(inString); 
     
        int i = 0; 
     
        do 
        { 
            printf("String[%d]: %c\n",i,inString[i]); 
        } while (inString[i++]); 
     
        return 0; 
    }

我们打印一个字符，检查它是否为零——所有的字符串都以之结束——并且如果它不为零，则打印该字符。需要注意的是，该类循环和其他的循环如 for 和常规 while 循环不同, while 条件语句之后需要加上一个分号。幸运的是，如果您碰巧忘记了这个要求，编译器将会产生错误回馈。
    
    foo.cpp: In function ‘int main()’: 
    foo.cpp:13: error: expected ';' before '}' token

在使用该循环执行任务时存在一个问题：如果用户没有输入任何内容而只是按下回车键，它将会打印出一个空字符。如果用户没有输入任何有效的内容，我们将会跳过打印。该任务使用 for 循环刚好，因为它需要很少的工作，但是我们可以添加一些代码来使其完成该任务。
    
    do
    {
    	// This will prevent problems caused by the user not typing anything
    	if (!inString[i])
    		continue;
    	printf("String[%d]: %c\n",i,inString[i]);
    } while (inString[i++]);

这里的 if 条件语句用于检测当前字符是否为0——使用 ! ，它和布尔逻辑运算符 NOT 相似。0 字符，通常代表假，它将会是 if 条件语句为真，那么程序将会执行 continue 语句。continue 使程序继续运行，直接进入循环体。引起跳转入循环条件的 0 字符将会导致条件为假，那么循环将会结束。

continue 语句的这种使用方式，可能不是很常见。这种情况下，通常会调用 break 语句，直接跳出当前的代码段。break 将会越过循环条件，然后直接结束循环。在接下来讨论的 switch 语句中，我们将会总是使用 break 语句。

## Switch语句

有些时候，我们必须从几个可用值之中选择一个选项用于某个变量。它可以使用一系列的if-else语句进行处理，但是C和C++为我们提供了可以更好的处理此种情况的switch语句。那么让我们把我们的字符串打印项目扩展为，提供一种方式打印字符，但是在打印时，不是用显示空格和缩进。

    #include <stdio.h>
     
    int main(void)
    {
    	char inString[1024];
    	printf("Type some text and hit Enter:\n");
    	gets(inString);
    	int i = 0;
    	while (inString[i])
    	{
    		switch (inString[i])
    		{
    			case '\n':
    			{
    				printf("String[%d]: <carriage return>\n",i);
    				break;
    			}
    			case '\t':
    			{
    				printf("String[%d]: <tab>\n",i);
    				break;
    			}
    			case ' ':
    			{
    				printf("String[%d]:<space>\n",i);
    				break;
    			}
    			default:
    			{
    				printf("String[%d]:%c\n",i,inString[i]);
    				break;
    			}
    		}
    		i++;
    	}
    	return 0;
    }

我们正在判断的值放置在 switch 语句的圆括号中。我们需要处理的每个值都放置在 case 语句中。这当我们要判断的值与 case 语句中的值相匹配时，这些 case 语句定义的语句集合，将会运行。Case 语句的格式如下所示：

    case valueForCase:
    {
    	block of instructions
    }

在这里的 break 语句非常重要，因为它们用户在处理了 case 语句之后跳出 switch 语句。如果在 case 语句结尾没有 break 语句，那么程序将会继续“运行”，并且继续进入下一个 case 语句。而这不是我们所希望的。使用上面的例子，删除 case 语句后面的 break 语句，者将会导致它打印两次：一次是空格 case，而另一次是 default case 语句。它的结果类似于下面的情况：

    String[5]: <space>
    String[5]:

default case 语句确保了不匹配指定情形中的所有值。它必须放置在 switch 语句中所有 case 语句的最后一个。任何放置在 default case 语句之后的 case 语句都将不被执行。虽然看起来模糊，但是它确实存在。

## 条件赋值

我们看到 C 和 C++ 为程序员提供了处理某些常用任务的快捷方式，例如添加 1 到变量。另一个快捷的方式是处理三项内容的单个操作符。下面是使用条件语句为变量赋值的方式：

    int number;
     
    if (someOtherNumber > 5)
    	number = 1;
    else
    	number = 10;

那么接下来是另一种方式，非常简短。

`int number = (someOtherNumber > 5) ? 1 : 10;`

那么，现在，您可能会想，“等一下，伙计！这完全没有意义？”，并且需要放弃幸运的马拉松赛跑。那么再次，可能不需要了。条件操作符具有两部分，问号标识和冒号。至于它的工作格式则如下：

`条件？条件为真时的值 : 条件为假时的值`

条件语句两端不需要圆括号，但是某些人（像我）喜欢这样做，即使根本没有必要把条件语句和其他部分分开。如果条件为真，那么在问好和冒号之间的值将会被返回，否则冒号之后的值将会被返回。虽然它的用处可能会有些限制，但是它某些时候确实比较方便实用。

## 找错

* 源码
        
        #include <stdio.h>
        #include <string.h>
         
        char *ReverseString(char *string)
        {
        	// This function rearranges a string so that it is backwards
        	// i.e. abcdef -> fedcba
        	if (!string)
        		return NULL;
        	int length = strlen(string);
        	int count = length / 2;
        	for (int i = 0; i < count; i++)
        	{
        		char temp = string[length - i];
        		string[length - i] = string[i];
        		string[i] = temp;
        	}
        	return string;
        }
         
        int main(void)
        {
        	char inString[1024];
        	printf("Type a string to reverse:");
        	gets(inString);
        	printf("The reversed string is %s\n",ReverseString(inString));
        	return 0;
        }

* 错误

    该程序的编译没有问题，但是没有打印出任何内容。

## 帮助
通常对于找错部分，我们是不给与帮助的，但是这个找错比较困难。该错误存在于 `ReverseString()` 中。尝试使用 `printf()` 调用打印特定位置的值来获取程序运行中的信息，像打印 length，count，等。