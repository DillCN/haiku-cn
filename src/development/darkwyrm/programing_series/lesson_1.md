# ��һ��

������Щ�Ѿ��Ķ��� Learning to Program with Haiku ������˵�����ǽ�������Ҫ�Ӵ���һЩ���ݣ����ǽ������������ C++ ������ѧϰ����ô�����ڱ�������Ҫ�����������ģ��ͱ�׼ģ��⣨STL�������������ߵ�ѧϰ����Ҫ���պܶ����Ϣ�����������Ҫ�����ȴ�Ľ��С�

## ģ��

ģ����һ�ֹ��ɳ������ڴ����κ��������ݵ�����ߺ����ķ�ʽ�����Ǵ�������ڴ��������࣬���磬��ջ���б�������������ǣ����ϣ���ܺõ�������������Ե����ƣ�������Ҫ��ʹ�����ǵ�����ߺ�����������һЩС���޸ġ�һ������ģ���������Ӧ��������ʾ��

    template <class T> 
    class ObjectArray 
    { 
    public: 
    				ObjectArray(const uint32 &size); 
    				ObjectArray(const ObjectArray &from); 
    				~ObjectArray(void); 
     
    	ObjectArray &		operator=(const ObjectArray &from); 
    	T &			operator[](int index); 
     
     	uint32			GetSize(void); 
    private: 
    	T 			*fData; 
     	uint32			fSize; 
    };

�����ʵ���У������ͳ������Ķ���֮�����Ҫ��ͬ���Ƕ���֮ǰ�� `template <class T> `��顣�ð����߱��������κ�ʱ�����ǿ���T��������ָ�ľ��Ǹ�������ʹ�õ����͡���Ҫע����ǣ����ǿ���ʹ���κε����������������������磬T��Key��Type��������������ϲ�������֣��������Ǳ���ʹ���� template ����������Ĺ����������������֡�ObjectArray ������������ԣ������� fsize ��Ԫ�ص��������� fData ָ������ڴ洢����Ԫ�صĶ��з��仺������һ�� ObjectArray ʵ������������ʾ����ʹ�ã������ص�������ʵ�֣�

    #include <stdio.h> 
    #include "ObjectArray.h" 
     
    int 
    main(void) 
    { 
    	ObjectArray<int32> intArray(10); 
     
    	for (uint32 i = 0; i < intArray.GetSize(); i++) 
    		intArray[i] = intArray.GetSize() - i; 
     
    	for (uint32 i = 0; i < intArray.GetSize(); i++) 
    		printf("intArray[%ld] is %lu\n", i, intArray[i]); 
    }

ʹ����ģ��Ĺؼ���ָ���������е����͡�������ʾ���У����Ǵ�����һ��ʹ�� int32 ������Ϊ���͵� ObjectArray ʵ���������ڴ��������� int32 ������صĹ��������������κ��������͡�һ�������������޷��ı����͡���һ�� `ObjectArray<int32>` ���ܹ��޸�Ϊ `ObjectArray<float>` ���� `ObjectArray<int16>`�����磬���������Ѳ�ͬ���͵� ObjectArray ������Ϊ��ȫ��ͬ�����ͣ�`ObjectArray<float>` ��`ObjectArray<int64>`����Ȼ���Ƕ�ʹ�� ObjectArray ����Ϊģ�壬�������ǲ���ͬһ���͡�

��һ������ģ������ж���һ���������Ǻ����ѡ���������������������Ҫ�������������������������������ඨ����Ϊ ObjectArray �ඨ�����к����Ĵ��룺

    template <class T> 
    ObjectArray<T>::ObjectArray(const uint32 &size) 
    	:	fData(NULL), 
    		fSize(size) 
    { 
    	if (fSize > 0) 
    		fData = new T[fSize]; 
    }
     
    template <class T> 
    ObjectArray<T>::ObjectArray(const ObjectArray &from) 
    	:	fData(NULL), 
    		fSize(from.GetSize()) 
    { 
    	if (fSize > 0) 
    	{ 
    		fData = new T[fSize]; 
    		for (uint32 i = 0; i < fSize; i++) 
    			fData[i] = from[i]; 
       	} 
    }
     
    template <class T> 
    ObjectArray<T>::~ObjectArray(void) 
    { 
    	delete [] fData; 
    }
     
    template <class T> 
    ObjectArray<T> & 
    ObjectArray<T>::operator=(const ObjectArray &from) 
    { 
    	if (this == &from) 
    		return *this; 
     
    	delete [] fData; 
     
    	fSize = from.GetSize(); 
    	fData = new T[fSize]; 
    	for (uint32 i = 0; i < fSize; i++) 
    		fData[i] = from[i]; 
    }
     
    template <class T> 
    T & 
    ObjectArray<T>::operator[](int index) 
    { 
    	return fData[index]; 
    } 
     
    template <class T> 
    uint32 
    ObjectArray<T>::GetSize(void) 
    { 
    	return fSize; 
    }

������Ҫע����ĳЩ�ط���Ӻ���ÿ����������ͷ����Ҫ��������֮�⣬��ʹ����ģ��ʱ������Ҫע��һ���ؼ������⣺����ģ��ĺ���������Ҫ������ͷ����������Դ�ļ��С�������Ϊ���������ں��������ģ�壬��������������Ķ��塣ʵ�ʵĶ����ڱ���ʱ��Żᴴ���������Щ������������Դ�ļ��У����������᷵����� undefined reference ���������˵�����ǲ����������е��ɻ�

��Щʱ��������Ҫ��������ʹ��ͬһ��ģ�塣������������Ҫ�Ľ�����һ������������Ҫʹ��ģ��ĺ������������ʵ���У�����Ҫ���Ľ����ǰ�ģ�����������ں��������ķ�������֮ǰ��������ʾ��

    class MyClass
    {
    				MyClass(void);
    				~MyClass(void);
    			void	SomeRegularFunction(int value);
    template<class T>	void	SomeTemplateFunction(T item); 
    };

�ڱ�ʾ���У�SomeTemplateFunction()�� MyClass ��Ψһʹ����ģ��Ĳ��֡����������ǵ� ObjectArray ���еĺ���������Ҫ��ͷ�ļ��ж��壬������ MyClass ����������һ��������Դ�ļ��С��������ʵ���У�ֻ�к���ģ����Ҫ������ͷ�ļ��С��������桱�ĺ���Ӧ�ú�ͨ��һ�����ڶ�����Դ�ļ��С�

## ʹ��ģ�壺��׼ģ��⣨STL��

ģ������ʵ�ʹ���������������������˵��ǣ�����������˵���ܶ����Ѿ�����ʱ��;�����������������ģ��������⣬��Ϊ��׼ģ��⣬��� STL���󲿷� Haiku API ���Ǿ�����Ƶģ�������ǲ���Ҫ������ʹ�� STL��������ȷʵ����һЩ��ʵ�õ����������� STL �е�������Ʒǳ���������˶���ʱ����Щģ���ʹ�ý����ֶ���ʹ����Щ�����������Ǵ���ʹ����Щģ����ࡣ

�κ�ʹ���� STL �� Haiku ��Ŀ��Ҫ���һ������Ŀ⡣Haiku �� GCC2 ������Ҫʹ�� libstdc++.r4.so �⣬�� Haiku �� GCC4 ����Ϊ��ʹ�� STL������Ҫ���ӵ� libstdc++.so��

�� STL �ṩ���������Թ�Ϊ���ࣺ˳�������͹���������˳���������ڴ����б��е�Ԫ�أ�������һ��ȷ����˳�������б��� Haiku �� API �ṩ�� BList ��Ҳ��˳��������һ�����ӡ���Щ������Ҫ���ڴ��������б�ÿ��һ��Ԫ�ء�����������Ҫ���ڴ�����Щ��Ҫ������߷�����ֵ��ѯ�����ݡ�������Ҫ������������õ�������ʹ���ַ�����ѯ���ݡ������ض�������ѡ��ȡ�������������ǵĹ�����ʽ��

### ����

ͷ�ļ���`<vector>`
vector��ǳ����������飬�������ǵ��ڴ����ʱ�Զ�����ġ������ǳ��ó����ٵ�ͨ��������ȡԪ�أ����κ�˳�����Ԫ�أ��Լ��ڽ�β��Ӻ�ɾ��Ԫ�ء�����ֻ�е����������ڲ��洢����������Ҫ�������ռ�ʱ�����Ԫ�ؿ��ܻ�Ƚ�����

### ˫����

ͷ�ļ���`<deque>`
˫���У�deque��ͨ������Ϊ��deck������˫����У�double-end queue���ļ�ơ�˫���зǳ��������������������ǿ�������β�������Ԫ�أ��������ǵ�Ԫ�ز���Ҫռ�������Ĵ���ڴ�ռ䡣�����Լ�����ȱ�㡣ʹ��ָ������ȡ�����е�Ԫ�ز���ȫ������������ͬ�����������Ǵ洢����Ԫ��ʱ�Ϻõ�ѡ��

### �б�

ͷ�ļ���`<list>`
�б�����Ϊ��̬����Ԫ�صļ��ϡ����Ƿǳ������ڴ���������룬ɾ�������ƶ�Ԫ�صȵĹ�����ʹ���б���Ҫ��ȱ���ǣ��޷����ٵĻ�ȡ�����Ԫ�ء�����ȡ�б��еĵ�ʮ��Ԫ����Ҫ�ӿ�ʼ��������������ʼ�㣩��������Ԫ�ء�

## �����ռ�

ʹ�� STL ������ҪһЩ��������룬��Ϊ���Ƕ�����װ�ڸ��Ե������ռ��С������ռ��Ǵ���һ���࣬�������������͵�һ�ַ���������ͨ�����ڷ�ֹ��ͬ���е�ͬ��������ĳ�ͻ��һ�������ռ����½���������

    namespace myNamespace
    {
    	int32 foo, bar;
    }

��ȡ�����ռ��е�Ԫ����Ҫָ���������ռ䡣���磬���е� SIL �������� std �����ռ��С�����һ�� int32 ���������������ʾ��

    // Note that there is no '.h' for this header and others in the STL. It's
    // just <vector>.
    #include <vector>
    std::vector<int32> intVector;

˫ð�������������ָ��ͬһ�����ռ��е�Ԫ�ز���Ҫʹ������ͬ���ģ���ȫ�������ռ���ָ��һ��Ԫ��Ҳ��Ҫ˫ð�š�

    #include <stdio.h>
    bool gSomeFlag = true;
    namespace myNamespace
    {
    	int intValue = 1;
    	void
    	SomeFunction(void)
    	{
    		// This specifies the gSomeFlag which is in the default (global)
    		// namespace
    		if (::gSomeFlag)
    			printf("myValue is %d\n", intValue);
    	}
    } // end myNamespace

ʹ�����������ռ��е����Ԫ�ؿ�����Ҫ�ܶ�����빤��������������Ҫ���ʹ��һ�������ռ䣬������ʹ�� using �ؼ�����������������빤����

    #include <deque> 
    #include <stdio.h> 
    // This eliminates the need to add the std:: before each reference 
    // to deque containers. 
    using std::deque; 
     
    int 
    main(void) 
    { 
    	// Declare our deque to accept integers. Without the using statement
    	// above, this would read std::deque<int> myDeque. Unless you're	
    	// using a lot of these, the using statement isn't really needed.
    	deque<int> myDeque; 
     
    	// Add one element to the end of the list which has a value of 5 
    	myDeque.push_back(5); 
     
    	// Print the number of elements in the deque. In this case, we're 
    	// definitely not playing with a full deque. 
    	printf("This deque has %d elements\n", myDeque.size()); 
     
    	return 0; 
    }

using �ؼ����ṩ�˾�ϸ�Ŀ��ƣ�������Ҫ�������������ռ䡣������ʾ���У�����������ʹ�ö�������ʱָ�������ռ����Ҫ���������ͬʱ��Ҫʹ������������������Ȼ��Ҫ��������ָ�� vector ���͵��õ� `std::vector<myType>`��������������ֳ�ͻ�Ŀ��ܣ�����������Ҫʹ��STL�е���಻ͬ������������ô���ǿ���ʹ�ø������� STL �����ռ�� using �������������У�

    using namespace std;

�������ַ�ʽʹ�� using �ؼ���ʱ��Ҫ�ر�ע�⡣������Ĵ�����Ҫ�����������ռ䣬�ǳ���������ÿ����ĵײ�ʹ���������򣬲�ʹ������Ȼ������������ڱ�дһ������API ��һЩ STL �� Haiku ������ôʹ�� using ��������ӿ����Ĺ��������ҽ���ǳ������塣

## STL������

STL �����������ʦ�ǳ����ǵ���ͼʹ���������� API �����ܵ����ơ����Ǵ����������ķ�ʽ֮һ�Ǵ��������������ڲ������е�������������һ��ʹ����������ȡ���е�Ԫ�أ����ʹ���˵�������

ÿ���������������һ������ʹ�õ�ָ��Ԫ�����͵�ָ�롣�������У�++ �� -- ������������������ָ����һ��������һ��Ԫ�ء��� for ѭ����ʹ��һ�� `vector<int>` ����������ʾ��

    #include <stdio.h> 
    #include <vector> 
    // This using statement only works for the vector class. It also makes
    // the loop code a little more readable.
    using std::vector; 
     
    int 
    main(void) 
    { 
    	vector<int> myVector; 
    	// Add a few values to our vector 
    	myVector.push_back(5); 
    	myVector.push_back(10); 
    	myVector.push_back(15); 
    	// The begin() method will always point to the first item in the
    	// container. end() will always point to the last one. This format
    	// works for *all* STL containers.
    	for (vector<int>::iterator i = myVector.begin(); i != myVector.end(); i++) 
    	{
    	printf("myVector: %d\n", *i); 
    	}
    }

�����Ѿ���������෽��������û�и����κεĽ��͡����ǲ��ҵ��ǣ��ܶ�ʱ������ʵ�����У����ڴ���Ŀ����ĵ����Ǵ��뱾���������������ǲ���Ҫ�����ַ�ʽ������ķ�����������������˫�б����б��Լ�һЩ�������õĶ���ʱ���������ġ�Ȼ����������Σ�������һ�������꾡���б�

<table border="1">
<tr> <th>����</th>                       <th>����</th> </tr>
<tr> <td>iterator begin();</td>          <td>����ָ�������е�һ��Ԫ�صĵ�������   </td> </tr>
<tr> <td>iterator end();</td>            <td>����ָ�����������һ��Ԫ�صĵ�������</td> </tr>
<tr> <td>size_type size();</td>          <td>����������Ԫ�ص���Ŀ��size_type��һ���޷������͡�</td> </tr>
<tr> <td>void push_back(const T &val)</td> <td>���ֵΪval��Ԫ�ص������б�Ľ�β����Ҫע����ǣ�������ʹ�κ��Ѿ����ڵĵ�������Ч��</td> </tr>
<tr> <td>void push_front(const T &val)</td> <td>ɾ�������е����һ��Ԫ�ء�</td> </tr>
<tr> <td>void pop_front(); </td>         <td>���ֵΪval��Ԫ�ص������б����ʼ����Ҫע����ǣ�������ʹ�κ��Ѿ����ڵĵ�������Ч���÷������������ࣨvector��������</td> </tr>
<tr> <td>void clear(); </td>             <td>ɾ�������е�����Ԫ�ء�</td> </tr>
</table> 

## ���ʹ�ã�ʹ��ʾ��

�ڿ�����Щ����֮�����Ƿ���û�����ʵ��������ʹ BList ���������ӵ����������磬ʹ������Щ����֮һ��ʵ�����ܻ��һ����׼�ķ��и�����Ч�ʡ�BList ��һ��Ŀ¼�д���һ�� entry_ref ������б�������һ��ʾ������չʾ����ο���ʹ���ǿ����Ĺ��� STL ������������ϢӦ�õ�����������ĵط���

    #include <deque> 
    #include <Directory.h> 
    #include <Entry.h> 
    #include <FindDirectory.h> 
    #include <Path.h> 
    #include <stdio.h> 
    using std::deque; 
     
    int 
    main(void) 
    { 
     
    	// Look up the home folder -- this is much preferable to a
    	// hard-coded way of accessing a system path.
    	BPath path; 
    	find_directory(B_USER_DIRECTORY, &path); 
    	BDirectory dir(path.Path()); 
     
    	deque<entry_ref> refDeque; 
     
    	entry_ref ref; 
    	while (dir.GetNextRef(&ref) == B_OK) 
    	refDeque.push_back(ref); 
     
    	printf("Contents of the home folder: %s\n", path.Path()); 
    	for (deque<entry_ref>::iterator i = refDeque.begin();
    	i != refDeque.end(); i++)
    	{
    	// Note that because an iterator can be treated like
    	// a pointer, we can access each entry_ref's name
    	// using the iterator. 
    	printf("\t%s\n", i->name); 
    	}
    }

## �����˽�

����޸�ʾ��Ϊʹ���б������˫���У�