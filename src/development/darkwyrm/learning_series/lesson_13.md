# ��ʮ����

����һ���У������˽���һ����̷�ʽ����Χ�ƶ������ƣ������Լ�������ִ��һ��������������Ҳ̽����C++�ж����֮Ϊ���ԭ�򣬶������ɣ��Լ�һЩ�����Ļ������ݣ�����Ϊ�˳�Ϊһ����Ч��Haiku������Ա��������Ҫѧϰ���ࡣ

## �̳�

Haiku ������Ա��д�ĺܶ���붼�ͼ̳���أ��̳м�����һ�����������ࣨ���ࣩ���ࡣ���磬�������ӵ��һ�������ࣨRectangle��������и߶ȣ�Height������ȣ�Width���������ṩ��һ�����ڼ��������Area�����ܳ�(Perimeter)�ĺ���������һ���������ࣨRectangularSolid����Ҳ�ǳ����ף��������һ�����ȣ�Depth����һ�����������Volume���ĺ������ɡ�

�����������ĵ��������������볤���ε�Ψһ������ô�Ͳ���ҪΪ���Ǳ�д���ڼ���������ܳ��Ĵ��롣������ǽ���ҪΪ���ǲ�ͬ�ĵط���д���룬�����̫�����ˡ��̳������Ϊ�˿��ܣ���������ͳ�Ϊ������������ࡣ�ͺ��Ӽ̳и�ĸ�Ļ���һ��������̳����丸������Ժͷ����������ǵĳ�������ͼ̳��˳�����������з��������ԡ�

<table border="1">
<tr> <td>    </td>   <td>������	   </td>       <td>������</td> </tr>
<tr> <td>����</td>   <td>Height�� Width</td>   <td>Height(�̳�)��Width(�̳�), Depth</td> </tr>
<tr> <td>����</td>   <td>Area��Perimeter</td>  <td>Area(�̳�)��Perimeter(�̳�)�� Volume</td> </tr>
</table> 

�̳��������Ǹ������еĴ��롣���븴���� C++ ��̵Ļ���֮һ���ٴ����ѣ�work smarter, not harder��

���ǽ���дһ�������ඨ�����ֱ�������������ӣ�

    class Rectangle
    {
    public:
    			Rectangle(int width, int height);
    		void 	SetWidth(int width);
    		int 	Width(void);
    		void 	SetHeight(int height);
    		int 	Height(void);
    		int 	Area(void);
    		int 	Perimeter(void);
    private:
    		int 	fHeight;
    		int 	fWidth;
    };
     
    // ���д���˵���� RectangularSolid ��Rectangle�����ࡣ
    class RectangularSolid : public Rectangle
    {
    public:
    			RectangularSolid(int width, int height, int depth);
     
    		// ���ǲ���Ҫ�г���Rectangle�̳������࣬
    		// ������Щ�³��ֵ��ࡣ
    		void 	SetDepth(int depth);
    		int 	Depth(void);
    		int 	Volume(void);
    private:
    		int	fDepth;
    };

�����������������ʹ�õķ��������ԡ���д�������ʵ�ֽ�Ҫ�����ǵĶ�����Ҫ�����˼�����������ǵ�һ�α�д�й���Ĵ��룬���ԣ���������ϸ�о�һ����Щ�����ע�͡�

    // Rectangle�Ĺ��캯���������Ὣ��Щֵ������������ԡ���Ϊ
    // ��ķ�����д����ʱ����������������ð�����ڷ�����֮ǰ��
    Rectangle::Rectangle(int width, int height)
    {
    	fWidth = width;
    	fHeight = heigh;
    }
     
    void
    Rectangle::SetWidth(int width)
    {
    	fWidth = width;
    }
     
    int
    Rectangle::Width(void)
    {
    	return fWidth;
    }
     
    void
    Rectangle::SetHeight(int height)
    {
    	fHeight = height;
    }
     
    int
    Rectangle::Height(void)
    {
    	return fHeight;
    }
     
    int 
    Rectangle::Area(void)
    {
    	return fWidth * fHeight;
    }
     
    int
    Rectangle::Perimeter(void)
    {
    	return (2 * fWidth) + (2 * fHeight);
    }
     
    // ������RectangularSolid���캯�����ڴ���RectangularSolid��
    // ͬʱ������һ��ʹ��Rectangle�Ĺ��캯��������һ��Rectangle����
    // ���ǽ�����height��width����ʹ��depth��ʼ��fDepth��
    RectangularSolid::RectangularSolid(int width, int height, int depth)
    {
    	fDepth = depth;
    }
     
    Void
    RectangularSolid::SetDepth(int depth)
    {
    	fDepth = depth;
    }
     
    int 
    RectangularSolid::Depth(void)
    {
    	return fDepth;
    }
     
    int
    RectangularSolid::Volume(void)
    {
    	// ���ǵ�����Width() ��Height() ������ʹ�� fWidth �� fHeight��
    	// ������Ϊÿ�����඼û�з��ʸö����˽�з��������ԡ�
    	return Width() * Height() * fDepth;
    }

��δ����֮ǰ������д�Ĵ���֮���������ͬ�����ڣ��Զ����˼��ǿ������ȥ���Ĵ������֯����д���գ����ŵĴ�����������Ĺ����ά���ǳ����ס�

����δ����У�Ψһ�Ƚ�İ���Ĳ���Ī������ Volume() �е��� Height() �� Width()����������ܹ����� fWidth �� fHeight�������Ǹ��ӷ��㣬����� protected ������ˡ����������ܲ�����Ҫ���߲��ᾭ���õ���

����һ��ֵ��ע��ĵط��Ǽ̳еĴ���Ҳ���� Rectangle �� public ���֡�����Ǽ̳е����͡�ͨ������ʹ�õĶ��ǹ�����public���̳С���Ҳ����˵�����෽�������Ե�Ȩ�޲��ᷢ���ı䡣ѡ�������������ͽ������ƶԻ���ķ��ʡ�ѡ�񱣻���protected���̳н�ʹ��������й������Ժͷ�����Ϊ protected ���� public����ʹ���Ƕ����е����඼���ã����Ƕ�����򲻿ɼ���˽�У�private���̳н�ʹ�������е����Ժͷ�����Ϊ private��ʹ����������κ� ��grandchild�� ��ȫ�޷����ʡ�

## �麯��

���಻����������µķ��������ԣ�����Ҳ�����޸����з�������Ϊ�����ǽ������������޸�ʱ���С����ڷ��������ķ���ֵ����ǰ��� virtual �ؼ��־��ṩ�����ֱ�֤��

    // ����������¶���÷�������Ϊ��
    Virtual void MyChangeableMethod(int someInt);
     
    // �������Ҫ�ض���÷�����
    Virtual void ThisMethodMustBeDefined(float someFloat) = 0;

��Ȼ����ı䷽������Ϊ��ʽ�������Ⲣ�������Ǹı䷽���Ĳ������ͺ��������Լ��䷵��ֵ���͡������еĳ�ʼ�汾Ҳ������ȫ��ʧ��������ͨ�����������������ָ����������ʾ��

    void
    ChildClass::DoSomething(void)
    {
    	Printf(��Child class did something\n);
    	ParentClass::DoSomething();
    }

## ��̬����

ͨ��������Ҫ���ö����ʵ�����ܹ�ʹ���䷽������ʱ����̫�鷳�ˣ��ѵ����еĺ������������䡰�����������ǿ�������ķ���ǰ����� static �ؼ���������á�ħ�䡱����̬�����������������麯��һ�����ڵ���ʱ��Ҫʹ���������������

    class Myclass
    {
    public:
    				    MyClass(void);
    		    int		DoSomething(void);
    	static 	int		DoSomethingStatic(void);
    };
     
    int
    main(void)
    {
    	MyClass myClassInstance;
     
    	// ���ú����ĳ��÷���
    	MyClassInstance.DoSomething();
     
    	// ����ĺ�������Ҫʵ�����ࡣ
    	// �ͳ���ĺ�����������΢�Ĳ�ͬ��
    	MyClass::DoSomethingStatic();
    	return 0;
    }

## ���أ�������ͬ���Ƶĺ���

ʹ��C���б��ʱ��һ�����ƣ�����Ӣ����Ϊ�˷��գ��κ��������������ܹ�ͬ������ʹ���ǵĲ���Ҳ����ͬ��C++ �Ƴ��������������Ϊ�������ܹ����ݲ��������������͵Ĳ�ͬ�������ǵ��������£�

    int MyFunction(int oneWay);
    int MyFunction(char *anotherWay);
    int MyFunction(float aThirdWay);

���ǣ��������������У�

    int SomeMethod(const char *oneConstString);
    int SomeMethod(const char *anotherString, const char *optionalString = NULL);

Ϊ�β����أ���������� optionalString ���������������޷����������ͼ��

## ϰ��

�Ķ� BeBook ���й� BApplication��Bwindow �� Bview �Ĳ��֡�������������еĶ����������뾡���������ио�������һ���У����ǽ�ҪΪ Haiku ��֯�����Ļ�Ȧ�ˡ�
