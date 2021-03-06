在有关 C++ 介绍的最后一节，我们将要学习另一种使用语言特性例外来处理错误，以及使用流来处理面向对象文件操作。

## C++ 输入输出流

C 开发者对于 open()，close()，fprintf()，以及其他一系列用以读取与写入文件和在屏幕打印文本的函数可能非常熟悉。而 C++ 开发者也拥有一组相似的语言概念，而且它们构成了更加清晰的文件操作。您可能还记得 Bstring 类重载 << 操作符以便为字符串添加信息。这种用法来自于 C++ 的文件操作方式。

C++ 和 IO 流的交互方式非常的自然。它使用双箭头操作符和流进行交互。`<<` 用于流的写入，而 `>>` 则用于流的读取。标准的管道 stdout，stdin，和 stderr 则相应地被 cout，cin，和 cerr 所取代，并且还添加了附加的用于日志的输出流，clog。但是它们的性能和使用并未发生改动。这些函数已经被写成了方法，并且可以在需要时进行重载。总之，C++ 的方式让您只需要学习很少的方法，而且它们要比相应 C 地方式更加的自然协调。

那么，我们来比较一下两者对文件的处理方式。下面的示例是一个 C++ 程序，其使用了 C 函数读取文件并将其打印到控制台。

	#include <stdio.h>

	int
	FileExists(const char *path)
	{
		// 该函数通过尝试打开文件来检测其是否存在。还有其他方法
		// 可以实现该功能，但是这种方法目前已经能够达到我们的目的。
		
		// 如果我们获取到的是一个NULL指针，那么我们需要返回 -1 以
		// 指明其为错误条件。
		if (!path)
			return -1;
		
		// 尝试打开文件
		FILE* file = fopen(path, “r”);

		// 如果文件存在，我们的返回值将是 1 ，如果不存在，则为 0 。如果
		// 打开文件出错，ferror() 将会返回非零值，如果正常打开，则返回 0 。
		int returnValue;

		// 如果给定 NULL 指针，ferror 将会报错。
		if (!file || ferror(file) != 0)
			returnValue = 0;
		else
		{
			fclose(file);
			returnValue = 1;
		}

		return returnValue;
	}

	int
	MakeTestFile(const char *path)
	{
		// 在处理字符串时总是检查 NULL 指针。
		if (!path)
			return -1;
		
		// 打开文件，如果其已经存在则清除其内容。
		FILE* file = fopen(path, “w”);

		if (!file || ferror(file))
		{
			// 如果我们无法创建该文件，那么我们将会得到不同的错误代码。
			// 这样可以让我们知道错误的原因是由无意传递的 NULL 指针，还是
			// 文件相关错误造成的。
			fprintf(stderr, “Couldn’t create the file %s\n”, path);
			return 0;
		}

		// 用于 stdout，stdin 和 stderr 的流句柄都已经定义好了，因此我们不需要额
		// 外的工作，如上述的 if() 条件语句，而可以直接使用它们。
		fprintf(file, “This is a file. \nThis is only a fine. \n”
			“Had this been a real emergency, do you think I’d”
			“be around to tell you?\n”);
		fclose(file);
		return 1;
	}

	int
	main(void)
	{
		int returnValue = 0;

		// 我们使用 /boot/home 中的测试文件 MyTestFile.txt。
		const char* filePath = “/boot/home/MyTestFile.txt”;

		// 如果文件不存在，则创建该文件。如果创建过程出现问
		// 题，则整体释出我们的程序。
		if (!FileExists(filePath))
		{
			returnValue = MakeTestFile(filePath);
			if(returnValue != 1)
				return returnValue;
		}

		printf(“Printing file %s:\n”, filePath);

		// 经历了这么多检测，现在可以安全的打印这个文件。
		FILE* file = fopen(filePath, “r”);

		if (!file || ferror(file))
		{
			fprintf(stderr, “Coundn’t print the file %s\n”, filePath);
			return 0;
		}

		char inString[1024];

		// fgets 在达到文件末尾时会返回 NULL 指针，因此这个小循环将
		// 会打印整个文件，直到文件末尾才退出。
		while (fgets(inString, 1024, file))
			fprintf(stdout, “%s”, inString);

		fclose(file);

		return 0;
	}

如果该程序以 C++ 来写，则如下所示：

	#include <fstream>
	#include <iostream>

	using namespace std;

	int
	FileExists(string path)
	{
		// 该函数通过尝试读取文件来检测其是否存在。还有更好的方法
		// 来完成这个任务，但是目前它已经能够达到我们的目的。

		if (path.empty())
			return -1;

		// 尝试打开文件以读取。
		ifstream file;
		file.open(path.c_str());

		// 如果我们的文件操作一切顺利，good() 将会返回真。
		return file.good();
	}

	int
	MakeTestFile(string path)
	{
		// 处理字符串时总是检测 NULL 指针。
		if (path.empty())
			return -1;

		// 打开文件，如果其已经存在，则清除其内容。
		ofstream outFile;
		outFile.open(path.c_str());
		
		// 检查我们可能出现的问题。
		if (!outFile)
		{
			// endl 常量比较特殊，其代表了行结束字符。通过使用 endl 常量
			// 来替代 ‘\n’ 序列，我们可以便利的编程而无需考虑它。
			cerr << “Couldn’t create the file ” << path << endl;
			return 0;
		}

		outFile	<< “This is a file. ” << endl
			<< “This is only a file. ” << endl
			<< “Had this been a real emergency, do you think I’d ”
				“be around to tell you?” << endl;

		outFile.close();
		return 1;
	}

	int
	main(void)
	{
		int returnValue = 0;
		
		// 我们使用 /boot/home 下的 MyTestFile.txt 作为测试文件。
		string filePath(“/boot/home/MyTestFile.txt”);
		
		// 如果文件不存在，则创建它；如果创建出现问题，则整个释出我们的程序
		if (!FileExists(filePath))
		{
			returnValue = MakeTestFile(filePath);
			if (returnValue != 1)
				return returnValue;
		}

		cout << “Printing file ” << filePath << “: “ << endl;

		// 经过这么多测试，现在可以安全的打印文件了。
		ifstream inFile;
		inFile.open(filePath.c_str())

		if(!inFile)
		{
			cerr << “Coundn’t print the file %s” << filePath << endl;
			return 0;
		}

		string inString;

		getline(inFile, inString);
		while (!inString.empty())
		{
			// getline() 剥离了行尾结束符，因此我们需要添加一个。
			cout << inString << endl;
			getline(inFile, inString);
		}

		inFile.close();

		return 0;
	}

虽然上述两者看起来不同，但是它们都完成了相同的任务。它们所不同的是潜在过程的多少。对于 istringstream 和 ostringstream 类，可以使用相同的接口来操作字符串。当然可以创建新的 iostream 子类来以新的方式操作，也可以重载 `<<` 和 `>>` 操作符以便将我们自己的类更好的操作 C++ 流。在许多环境中，我们可以利用那些在后台进行内存管理的方法，例如 getline() 。接下来，我们来看一下 istream 和 ostream 类提供的可用方法。

## 输入流（istream） 方法

	operator >>
从流中取出信息，类似于 fscanf() 和 sscanf()。


	streamsize gcount() const;
返回 get() 和 read() 操作期间最终所读取到的字节数。

	int get();
	int peek();
以上两个方法从流中获取一个字符，并将其返回。peek() 操作时不移动流中的读取指针。


	istream& get(char& c);
从流中获取单个字符，并将其存放在 c 中。

	istream& get(char* string, streamsize count);
	istream& get(char* string, streamsize count, char delimiter);
从流中读取字符直到匹配相应的条件：其读取到了第 count -1 个字符，到达了文件的末尾，或者是在具有 delimiter 的条件下，遇到了 delimiter 中的字符。

	istream& getline(char* string, streamsize cound);
	istream& getline(char* string, streamsize cound, char delimiter);
从流中读取一行，直到第 count 个字符，或者遇到 delimiter 字符。

	istream& read(char* buffer, streamsize count);
从流中读取count个字节，除非到达文件末尾。

	streampos tellg();
	istream& seekg(streampos position);
	istream& seekg(streamoff offset, ios_base::seekdir direction);
以上方法获取或者设置下一次 get() 调用的位置。这个位置可以是绝对位置或者相对位置（offset，direction；偏移量，和偏移方向）。

## 输出流（ostream ）方法

	operator <<
将格式化文本写入流，类似于 fprintf()，sprintf()。

	ostream& put(char c);
将字符写入流。

	ostream& write(char *string, streamsize count);
将长度为 length 的字符写入到输出流。

	streampos tellp();
	ostream& seekp(streampos pos);
	ostream& seekp(streamoff offset, ios_base::seekdir direction);
以上方法获取或者设置下一个 put() 或 write() 调用的位置。该位置可以用绝对位置或者是相对位置。（相对于当前位置的偏移量和偏移方向，即 offset 和 direction）。

## 输入输出流通用的方法

	bool good() const;
	bool bad() const;
	bool fail() const;
	bool operator ! () const;
	bool eof() const;
以上方法用于处理流的错误状态。当文件流到达结束位置时，需要设置 eof 标志，其状态由 eof() 函数返回。当遇到影响到流连续性的错误时，将会为其设置 bad 标志。当单个操作由于某些原因失败，而且因为某些影响到常用操作的问题而设置了 bad 标志时，将会设置 fail 标志。设置 bad 标志时，bad() 将返回真；当设置了 bad 或 fail 标志时，fail() 返回真。操作符 ! 完成同样的工作。good() 并不是 bad() 的反面，遇到任何失败标志，如 bad，fail，eof 等，它都会返回假。简而言之，在读取文件时，`while(myStream.good())` 能够充分保证您可以读取该文件。

	streamsize width() const;
	streamsize width(streamsize wide);
获取或者设置区域宽度。如果您希望以对齐或者等宽方式进行打印，您将需要该方法。

	char fill() const;
	char fill(char c);
设置或读取用于对齐或者等宽格式的填充字符。默认为空格。

细数之下，方法可真多。不过，对于基本的文件操作，如读取或者写入，仅需要很少的一部分方法。但是上述的这些方法还未完全覆盖所有可用的方法。它们只是您在日常编码中可能需要用到的方法。

## 格式化C++流

C++ 流所提供的易用性之一就是格式化输出。printf() 及其兄弟们提供了很丰富的格式化选项，但是 cin 和 cout 提供了更多。它们使用的方式都相同，使用 endl 作为行结束符。

多数流操纵符与 endl 不同，它们将实际的修改其所影响的流的状态。例如， boolalpha 操作符将使布尔值转换为它们的字符串等价形式。鉴于它们通常都会被转换为其数值等价的字符串形式（1对应于true，0对应于false），这是一个非常好的情况。如果将 boolalpha 操纵符发送到流中，那么之后所有的布尔值都会被转换为 “true” 和 “false”。当然也可以发送 noboolalpha 操纵符到流中将其关闭。

	#include <iostream>
	using namespace std;

	int
	main(void)
	{
		// hex 操纵符将使整数以十六进制的形式显示
		cout << boolalpha << hex;
		cout << true << endl;
		cout << 123 << endl;

		// 操纵符也可以如下所示内联得发送，也可以如上述方式发送。
		cout << noboolalpha << true << endl;

		return 0;
	}

该程序的输出结果如下：

	true
	7b
	1

下面是其他可用的操纵符列表。

<table border="1">
<tr> <td>操纵符</td><td>描述</td> </tr>
<tr> <td>boolalpha, noboolalpha</td><td>开启/关闭布尔值与等价字符的转换，如 1->"true" </td> </tr>
<tr> <td>dec, hex, oct	       </td><td>设置数值模式为十进制，十六进制，或者八进制</td> </tr>
<tr> <td>flush	               </td><td>刷新文件缓冲区，任何等待写入流的数据都将被写入</td> </tr>
<tr> <td>skipws, noskipws	   </td><td>切换跳过空格。启用时，它将会导致读取时跳过制表符，空格和换行符。在读取配置文件时，该模式会节省很多时间</td> </tr>
<tr> <td>showbase, noshowbase  </td><td>根据数制，显示前缀，十六进制前缀为0x，八进制为0， 十进制没有前缀</td> </tr>
</table>

上述表格并不完整。在任何 C++ 参考文件中都有更加完整的说明，您也可以参考 [http://www.cplusplus.org/](http://www.cplusplus.org/)

## C++ 异常(Exceptions)

异常是在我们的程序中构建错误处理方法的一种方式。因为它们会对性能造成不必要的影响，在 Haiku 中通常并不会使用，但是对它们有一定的了解还是非常重要的。

异常的使用主要围绕 C++ 语言中三种不同的元素：try 代码段，throw 语句，以及 catch 代码段。当一段代码可能会进入例外错误条件时，就将其放入 try 代码段。在出现问题时，它就会抛出一个异常。这样执行程序将会调用一系列嵌套的函数调用，即 call 栈，直到它找到了用于处理此类例外的 catch 代码段。如果它到达了 call 栈顶，但仍未找到相应的处理函数，那么您的程序将会意外退出。下面是异常使用的实例：

	#include <iostream>

	using namespace std;

	void
	SomeFunction(void)
	{
		// 假定在这里遇到了意外，我们将抛出一个异常。
		throw 10;
	}

	int 
	main(void)
	{
		try
		{
			// 我们将可能会产生异常的程序放到 try 代码段中。万一出现问题，它将会在
			// catch 代码段中进行处理。
			SomeFunction();
		}

		catch (int error)
		{
			// 一旦异常到达 call 栈顶，它将会使程序完全退出，
			// 这也是我们不希望发生的。如果您有一个 try 代码段，
			// 您应该在其后加上相应的 catch 代码段。
			cout << “An unusual error occurred, Exception number ”
				<< error << endl;
		}
		return 0;
	}

在 Haiku 编程过程中，很少用到异常，因为其 API 提供了足够的错误处理功能，并且如前所述，异常将会导致严重的性能问题。

## 深入了解


* 查阅其他参考资料中，了解更多的操纵符。怎样加以利用可以产生很好的效果？
* 如果您希望设计一个简单的内存数据库，并且其记录具有固定的大小，您该如何表述各种数据类型？您可以使用哪些 STL 容器？如何读取，写入，以及删除记录内容？如何保存和载入其内容？