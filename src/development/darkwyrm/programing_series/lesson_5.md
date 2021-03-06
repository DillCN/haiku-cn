我们要花点时间把我们已经学过的不同代码凑到一块。从第一节课开始，我们先后学习了下面几部分内容：

* 模板（Template）
* 命名空间（Namespace）
* 迭代器（Iterator）
* C++ 字符串类（string class）
* STL 关联容器：map，set，multimap，以及 multiset
* STL 顺序容器：vector，deque，和 list
* STL 容器适配器：queue 和 priority_queue
* C++ 输入输出流，即 cout 等
* 异常(Exceptions)


以上这些内容，已经足够写厚厚的一本书，而且对标准模板库和 C++ 标准库的高效使用做一个透彻的讲解。在这里，高级应用并不是我们的目的，我们主要是希望能够对这些话题有一个全面的了解以便于我们在编写 Haiku 应用时能够写出更好的代码。

## 项目：读入 Paladin 项目

对于不了解的人来说，Paladin 就是 Haiku 上可用的集成开发环境之一。它的界面设计与原 BeIDE 非常相似，而 BeIDE 是彼时 BeOS 上主要的 IDE。Paladin 与 Haiku 上其他的IDE 相区分的主要特性是它独特的项目文件格式，特殊格式的文本文件。这使得它可以方便地进行导入和导出。

这种格式本身相对简单。每个文件都是一个列表，每行都是一个键值对。这也使得它可以连续的读入和解析项目文件。因为新的键会被老版本的程序忽略，这也增强了它的前向兼容性。

本项目是一个跨平台的项目阅读器。我们将要使用 STL 和 C++ 标准库，以保证它可以在任何的操作系统上可用。该项目主要可用作项目转换程序的一部分，用于将 Paladin 项目文件转换为 makefile，Jamfile，shell 脚本，以及其他构建系统。

	#include <fstream>
	#include <iostream>
	#include <list>
	#include <map>
	#include <string>
	#include <strings.h>
	#include <vector>

	// 因为我们需要多次使用 std 命名空间，如下使用可以节省很多次输入呢。
	using namespace std;

	// 虽然以下的结构体可以并且应该用作类，但我们仅使用结构体以简化程序
	// 和节省空间。
	typedef struct
	{
		// 文件路径可以存储为绝对路径，即其以 / 作为起始，或者相对于
		// 项目文件位置的相对路径。
		string path;

		// 依赖文件存储为文件路径列表，彼此之间以管道符（|）分开。
		string dependencies;
	} ProjectFile;

	typedef struct
	{
		string name;
		bool expanded;
		vector<ProjectFile> files;
	} ProjectGroup;

	// Paladin 实际使用的项目类将更加复杂，因为它有一些在程序
	// 运行时管理状态的属性，但是这个结构体已经具有了所有用
	// 于构建和管理项目的数据。
	typedef struct
	{
		map<string, string>		properties;
		vector<string>			localIncludes;
		vector<string>			systemIncludes;
		vector<ProjectGroup>		groups;
		vector<string>			libraries;
	} PaladinProject;

	int
	ReadPaladinProject(const char *path, PaladinProject &proj)
	{
		// 创建输入文件流以读取文件。
		ifstream file;
		
		file.open(path, ifstream::in);
		if (!file.isopen())
		{
			// endl 是跨平台的行结束符。而 EOL 字符在 Windows，
			// Haiku/Linux/Haiku，以及 OSX 上则不同，因此它省去
			// 了我们深究它的不同，而且还不失去可移植性。
			cout << “Could’t open the file ” << path << endl;
			return -1;
		}

		// 清空项目数据，以确保我们没有在已存在的项目上进行构建。
		proj.properties.clear();
		proj.localIncludes.clear();
		proj.systemIncludes.clear();
		proj.groups.clear();

		while (!file.eof())
		{
			string strData;
		
			// 虽然 fstream 类具有 getline() 方法，但是它仅作用于常规字符串。
			// 在 <string> 中有一个全局 getline() 函数可用于从流中将数据读入
			// C++ 字符串。如下是我们使用的方法。
			getline(file, strData);

			// 在 Paladin 项目中不存在空行，但是我们需要处理这种异常情况，
			// 以避免让人头痛的事情。
			if (strData.empty())
				continue;
		
			size_t pos = strData.find(‘=’);
		
			// npos 是 C++ 字符串的最大长度。如果查找的字符串未找到，find()
			// 将会返回 npos 。
			if (pos == string::npos)
				continue;
		
			string key = strData.substr(0, pos);
			string value = strData.substr(pos + 1, string::npos);
		
			if (key.compare(“GROUP”)) == 0)
			{
				// vector 容器可以让我们免于为内存管理和指针而纠结。而且，
				// 我们所需要做的只是在栈上新建一个组，并使用它类初始化
				// 组的 vector 中的新元素。
				ProjectGroup newGroup;
				newGroup.name = value;
				proj.groups.push_back(newGroup);
			} 
			else if (key.compare(“EXPANDGROUP”) == 0)
			{
				if (!proj.groups.empty())
					proj.groups.back().expanded = strcasecmp(value.c_str(), “yes”);
			}
			else if (key.compare(“SOURCEFILE”) == 0)
			{
				// 很多点用于创建新文件，但是没关系。它确实很容易和指针相混淆。
				ProjectFile newFile;
				newFile.path = value;
				proj.groups.back().files.push_back(newFile);
			}
			else if (key.compare(“DEPENDENCY”) == 0)
			{
				proj.groups.back().files.back().dependencies = value;
			}
			else if (key.compare(“LIBRARY”) == 0)
			{
				proj.libraries.push_back(value);
			}
			else
			{
				proj.properties[key] = value;
			}
		}
		
		return 0;
	}

	int
	main (int argc, char **argv)
	{
		PaladinProject project;

		if (argc == 2)
		{
			ReadPaladinProject(argv[1], project);
		}
		else
		{
			cout << “Usage: ” << argv[0] << “ <path> \n”;
		}
		
		map<string, string>::iterator i;
		for (i = project.properties.begin(); i != project.properties.end(); i++)
		{
			cout << i->first << “: ” << i->second << endl;
		} 

		return 0;
	}

## 深入了解

* 使用本项目作为打印 Paladin 项目信息的起点。
* 创建一个项目，读取 Paladin 项目文件，并且将其分割为 makefile 或者 Jamfile 。
