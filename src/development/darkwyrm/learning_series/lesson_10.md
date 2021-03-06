# 第十课

当我们第一眼看到指针时候，我就觉得它们是非常强大的工具。今天我们就来学习一下如何在函数中使用它们，还有一些给函数传递参数的不同方法。

## 参数的默认值

当我们调用函数的时候，会发现有时候某些参数几乎不会有什么变化，比如说我们正在写一段在屏幕上创建一个窗口的函数：

    void MakeWindow(int left,int top,
                    int right,int bottom,const char *title,
                    int type,
                    int look);
				
每次我们调用这个函数的时候，窗口的位置，大小和标题可能都各不相同，不过类型和外观很可能不太会变化，几乎每一次 type 和 look 的值都一样因为我们只是生成普通的窗口。每次都输入会浪费时间，所以我们就给这这两个参数定义个默认值。我们通过小小的修改声明文件来完成这个工作。

    void MakeWindow(int left,int top,
                    int right,int bottom,const char *title,
                    int type = REGULAR_WINDOW_TYPE,
                    int look = REGULAR_WINDOW_LOOK);
				
现在我们已经为这两个参数设置了默认值，这样一来每当我们调用这个函数的时候就不必都要给这两个参数传值了。有默认值的参数都要放在参数表的最后。在这个例子中，当要创建普通窗体时就节省了很多打字工作。

    MakeWindow(100,100,500,400,
               "MyWindow",
               REGULAR_WINDOW_TYPE,
               REGULAR_WINDOW_LOOK);
    		   
就可以简写成

`MakeWindow(100,100,500,400,"MyWindow");`

在我们开始编写和 Haiku 的图型包有关的程序之前我们不会看到太多用到默认值的实例，但早点了解还是有帮助的。

## 引用

这里说的引用，不是某些你可以从库里找到的东西，它们其实是指针和普通变量的一种混合体，使用中它被看作是一个普通的变量，不用在前面加个星号才能读出它的值，不过它们本身有没有存储数据的空间，而是使用其他变量的存储空间。这里有个声明引用的例子：

    int myInt;
    int &myRef = myInt;

这里在 int 和 myRef 中间的 (&) 符号就是声明普通变量和声明引用的不同点，在这个例子中，myRef 是 myInt 的一个引用，myRef 是，简单说来，就是 myInt 另外的一个名字。改变它们中一个的值，另一个也会跟着改变，这对于存储了有效地址的指针也是一样的，看下列代码：

    #include <stdio.h>
    #include <malloc.h>
     
    int
    main(void)
    {
        //创建一个指向堆内存空间的指针并初始化它
        int *myPointer = (int *) malloc(sizeof(int));
        *myPointer = 1;
     
        //创建一个myPointer指向空间的引用
        int &myRef = *myPointer;
     
        myRef++;
     
        printf("The value at myPointer's location is %d\b",*myPointer);
        free(myPointer);
     
        return 0;
    }

现在引用可能会让你想说“好吧，这玩意挺好，我们继续”，不过还有一些东西得让你知道，首先，你不能改变一个引用的指向。就是说 myRef 将永远是 myPointer 指针指向的那块内存的引用，即使 myPointer 指针自己都变了。所以如果在 myPointer 被释放后还试着使用 myRef 会引发段错误。这个不可变性其实是很有益的。一个引用必须被初始化，所以它就一直都有效，除非它引用的内存被释放或者它引用的变量出了自身的生命周期。这让引用相对指针来说更安全而同时也带来了同样的灵活性。

## 参数使用：传引用还是传值

从函数外获取数据是有不止一种方法的。最普通的方法是传值，不过引用提供了另外一种方法。一般的，当一个参数被传入函数时，是变量的一个副本被传了进去 -- 是参数的“值”被传入函数，而不是变量本身，这意味着，你可以完全不考虑在这个函数外是否会造成麻烦的随意修改这个参数。当一个参数的引用被传入函数，那在这个函数对这个参数所做的一切更改会持续到这个函数退出之后。让一个函数用引用来传递参数只需要简单在参数名前加个(&)符号。

    const char * someFunction(int &integerByReference);
    float someOtherFunction(const double &doubleByReference);

在这个例子里，someFunction 函数不仅返回一个字符串，而且它还可以修改 integerByReference 的值，这就相当与让我们可以通过一个函数得到两个返回值。someOtherFunction 使用了一个常引用 const reference，可能这看上去有点傻--传入一个我们不能更改的引用。但这么做其实是有目的的：它节约了一个副本。这是让一个经常被调用的函数跑的更快的一种方法，特别当这个函数有很多参数或者其中的某个参数占很大内存空间的时候。

现在来让我们确定一下我们的确已经了解了值传递和引用传递的区别。让我们来看一段能说明这个区别的代码：

    #include <stdio.h>
     
    //这个函数的x参数通过引用传递，y通过值传递
    int
    myFunction(int &x,int y)
    {
        x = x * 2;
        y = y + 5;
        return x * y;
    }
     
    int
    main( void )
    {
        //为了测试来创建两个变量
        int foo = 5;
        int bar = 10;
     
        int outValue = myFunction(foo,bar);
     
        printf("foo is %d,bar is %d,and myFunction(foo,bar) is %d\n,
                   foo,bar,outValue");
     
        return 0;
    }

在这个例子中 foo 开始是 5，但由于 myFunction() 改变了它，在打印出来的时候它的值已经变成了 10，bar 的值并没有变化因为 myFunction() 函数其实只是改变了 bar 的一个副本的值。

## 函数指针

就在你觉得指针已经没法更怪异的时候，它就更怪异了。指针不仅能够指向存储值的内存，它还能指向存储代码的地址。

    void (*functionPointer)(int value,int anotherValue);

上面这行代码不是函数，它实际上一个叫做 functionPointer 函数指针的类型是很特殊的，返回值，参数的个数和类型都是函数指针类型的一部分。下面两个函数指针就不是一个类型的:

    void (*integerFunction)(int value);
    int (*anotherIntegerFunction)(int value);

用指针的方式运行一个函数简单到死：把指针当作函数名就行了。下面这个例子就调用了 integerFunction 指针所指向的函数：

    integerFunction(5);

就好像引用一样，指针函数的用处到目前来讲没那么明显。但它实际上能给程序带来难以置信的弹性。代码可以被拆上拆下的就好像汽车一样。对于类似Perl或者Python这类解释型语言，更改程序是很简单的，不过对于编译型语言比如C++就没那么容易了。尽管不太多见，我们会在日后学习程序插件的时候使用到一些函数指针。现在，不用多去管它。

## 指针的指针

是滴，指针还可以指向其他指针。一个指针存储的内存地址完全可以是另一个指针自身的地址。其实只要在声明指针的时候加上第 2 个星号就可以了。

    char **somePointerToAPointer;

不要忘记指针的声明是不会申请空间的。这个声明完成后唯一存在的是一个叫 somePointerToAPointer 的指针。我们可以把它用在很多事情上，比如从函数返回一个指针但不用返回值或者创建一个字符串列。是滴，这就是你在堆上创建一个字符串列的方法。这也是我们从命令行里得到参数的方法。

## 命令行参数

就好象函数有参数，程序本身也可以接受传给它们的某些信息，让我们拿下面的终端命令做为例子：

    $rm -f --verbose myFile

这个命令 rm 有三个参数：一个文件名和 2 个开关 switches。命令行开关 command line switches 是改变程序行为的属性选择。在这个例子中，rm 作用与 myFile 之上。-f 开关告诉它要强制删除，不用确认。--verbose 开关告诉它要打印出比平时多的执行信息。

在 Windows 中开关以斜杠开始，而在 Linux,OS X 和 Haiku 里，它由中划线开始。还有默认的规则，一条中划线的开关只跟一个字母，而两条中划线的开关后可以跟由一条中划线分割开的词语或者短语。我们不会在课程里花太多的时间来关注命令行开关，因为我们的项目一般不会复杂到要使用它们。

为了能让程序占到命令行参数的便宜，我们必须改变我们写 main() 函数的方式：

    int
    main(int argc,char **argv)
    {
        return 0;
    }

现在 main 函数有两个参数：argc 是从命令行中读取的参数的个数，argv 则是一个字符串列表，转载了命令行参数。把 argv 当成是一个数组-- 如果 argc 等于 2,那 argv 就有两个元素标记是 0 和 1。`argv[0]`永远是当程序运行时存储这个程序名字的地方。下面这个程序将打印被传递给它的命令行参数：

    #include <stdio.h>
     
    int
    main(int argc,char **argv)
    {
        for(int i = 0; i < argc; i++)
    	printf("Program argument %d: %s\n",i,argv[i]);
        return 0;
    }

要注意的地方是 printf() 的最后。使用指针的指针就好像是在使用多维数组。string 只不过就是一个特殊的 char 数组。所谓我们这里就逐个的访问字符串。如果我们指向使用第一个参数的第二个字符。我们可以使用 `argv[0][1]`。这里可能会搞混，方括号顺序是从左到右从最大组到最小组。所以是在访问列表的列表中的第 0 个元素中的第 1 个元素--也就是那个列中的第 2 个字符。

## 项目

学到现在，我们已经有能力利用我们所学到的一切来做出点东西了。这将会是我们第一个有价值的项目。我们要写一个 cat 命令的简略版本。文件要依照它们被输入到命令行里的顺序被依次打印到标准输出 stdout 里。

为了完成这个项目，我们还需要两个新函数,fread 和 fwrite:

    size_t fread (void *buffer,size_t size,siez_t count,File *stream);
    size_t fwrite(void *buffer,size_t size,size_t count,File *stream);

fread 从文件流中读取数据，这个函数将尝试读取 `size*count` 字节的数据，并把它们放在 buffer 中。这个函数非常好使，因为你可以为 buffer 申请任意类型的数组。使用 sizeof() 函数确定类型的大小给 size，然后设定数组里元素的个数给 count，fread 返回真正被读取的元素的个数。如果这和被要求的个数不一样，那就要么有错误发生，要么就是文件读到头了。

fwrite 和 fread 工作模式很像，但是是逆向的，在 buff 中的数据会被写到 stream 里，实际被写入流的元素的个数被作为返回值返回。

总体说来，这个项目包含了我们刚学到的如果使用命令行参数，结合第8课里的文件操作加上 fread 和 fwrite。使用 for 循环来为每个命令行参数执行下面这一连串操作：

1. 尝试将参数当文件打开用于读取
2. 如果打开错误，跳到下一个循环
3. 如果打开成功，尝试读取文件的一块，把读取的字节数储存在一个变量中
4. 使用while循环读取文件内的数据，当读取的字节数大于0的时候不停的循环
5. 把读取的字节数打印在标准输出里
6. 尝试从文件流中读取更多的数据，并保存实际读取的字节数
7. 关闭文件流

提示，警告和建议：

* 使用ferror来打印文件错误是一个很好的尝试
* 用来储存文件数据的内存块可以来自堆也可以来自栈


因为我们还没有真正写过什么代码，所以我来帮你入一下门。你要做的就是把每行评论都替换成真正的代码就行了。每次写一点编译一点是个很好的做法。一步步的写代码，测试代码有助于定位 Bug 和让 Bug 最小化

    #include <stdio.h>
    #include <malloc.h>
     
    int
    main(int argc, char **argv)
    {
        for (int i = 1; i < argc; i++)
        {
    	// 为了从argv[i]中读取数据打开一个文件流
    	// 如果文件流为NULL或者有错误，则进入下一个循环
    	// 创建一个数据缓冲 -- 一个存放我们数据的数组，大小不是很重要
    	// 但它至少应该有几百字节大,但不要大过4000字节
    	// 你可以把它创建在栈上
    	// 或者使用malloc，你爱怎么做都可以
    	// 创建一个变量来存储真正读出的字节的个数
    	// 从文件流里读取数据并存储读出的字节个数
    	// 读取我们刚建立的变量的值
    	// 开始while循环，如果读出的字节的个数大于0,
    	// 并且文件流没有出现ferror错误，则一直循环
    	{
    	    // 把读取的字节个数输出到标准输出stdout里
    	    // 读取更多的数据并更新读取的字节个数
    	    // 到我们上面创建的那个变量里
    	}
    	// 如果你使用了malloc，则释放缓存
    	// 如果你使用的是栈空间，就不要管了
    	// 关闭文件流
        }
        return 0;
    }