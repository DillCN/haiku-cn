# Haiku����淶

���Ĳ��Ǻ����������ĳһ�������͵Ĺ淶������û��д������Ӧ����ѭ�Ѿ�д�õĴ���ķ�����ʹ�� Haiku ��Դ������Ϊ���ĵĲο����������Դ��и��õĽ��飬�����ֱ����[����](http://www.haiku-os.org/contact)˵��������������ṩ���ơ��Ҹ�ϲ���������͵�����������ǿ��Ըĳ�������ô�����Ľ��顣

����ʱ��ԭ�򣬿�����ĳЩ�ط���һЩ�����������ǵ�Դ��淶��һ�¡�������ܹ���æ����Щ��������������ǽ���ǳ���ӭ��

�����ָ��һ���ض��Ĵ��������Ǹ���Ҫ��Ŀ�����ڱ�֤�����ǰ��һ�£��ھ���һϵ�е��ⲿ���빱�׺Ͳ����ύ֮������ϣ��Haiku�Ĵ����ܹ���Ȼ�������࣬һ�£����������Ķ���ά����

## �淶�ſ�

����������ʹ���Ǻ��������׵��������뱣�ֱ���ǰ��һ�¡������ύ������ʱ����ῴ����һ����ᴩ��������淶��

## �����Ϳո�

* ʹ���Ʊ������������������ı༭����ÿ���Ʊ������Ϊ4���ո�
* Haiku����������� [K&R](http://en.wikipedia.org/wiki/Indent_style#K.26R_style)���ǳ��ӽ���
* �������ռ���ĺ���/�಻��Ҫ����������
* ͨ����ÿһ�����ʽǰʹ��һ���Ʊ��������Բο���������ӡ�

���ȣ�������ͨ��һЩ��������ʶ��Ҫ�Ĺ淶��

    class Foo : public Bar {
    public:
                                        Foo(int32);
        virtual                        ~Foo();
 
        virtual	void                    SomeFunction(SomeType* argument);
 
        // ʹ�� tab �����Ƴ������б��������:
        virtual	const char*            FunctionWithLotsOfArguments(const char* name,
                                            const char* path, const char* user);
    private:
                int32                    _PrivateMethod();
        static	int32                    _SomeStaticPrivateMethod();
 
                int32                    fMember;
                const char*              fPointerMember;
    };
     
     
    // ð�� ':' ��������һ��,֮����ǳ�ʼ��������
     
    Foo::Foo(int32 param)
        :
        Bar(int32* param),
        fMember(param),
        fPointerMember(NULL)
    {
	...
    }
     
     
    /*!	��������ͨ��ʹ�� doxygen ��ʽ����Ҫע����ǣ� �����ע��
	������Ϊ�ն��û���Ƶ��ĵ����������ڰ���������ȷʹ��
	�öδ�����ĵ���
    */
    template<class T>
    const T*
    Foo<T>::Bar(const char* bar1, T bar2)
    {
    	...
    }

    static int32
    my_static_function()
    {
        return 42;
    }

    if (someCondition) {
        DoSomething();
            // ���ڵ��е�ע��ͨ��д�ڴ�����֮�󣬲���Ҫ����һ�У�	
            // ���һ�Ҫ����һ���Ʊ����������
        DoSomethingElse();
    } else if (someOtherCondition) {
        DoSomethingElse();
        DoSomething();
    }
 
    if (someVeryVeryLongConditionThatBarelyFitsOnALine
        && someOtherCondition) {
        // && ������Ҫ������һ�е���ʼλ��,
        // ����Ҫ����һ���Ʊ��������
        ...
    }
 
    if (someVeryVeryLongConditionThatBarelyFitsOnALine
        && (someVeryLongNestedConditionPart1
            || someVeryLongNestedConditionPart2)
        && lastPartOfSomeVeryVeryLongCondition
            != 0) {
        // ͬһ���ȼ��Ĵ��뱣����ͬ��������
        ...
    }
 
    if (fMemberPointer->VeryLongFunctionCall(uint32 argument1,
        uint32 argument2, uint32 argument3) != NULL
        && someOtherCondition) {
        // �������õ�Բ���ŵ�����һ�����ȼ�
        // ��������Ҫ�ڵڶ�������һ���Ʊ���Ķ�������
        ...
         
        localVariable = AnotherLongFunction(uint32 argument1,
            uint32 argument2, uint32 argument3);
                // ��������򵥵ķ�������һ�������
                // �������ڴ���Ŀɶ��Բ�û��̫�����
            
        anotherVariable = fSomeUselessRGBColor.alpha *
            (fSomeUselessRGBColor.red + fSomeUselessRGBColor.green
                + fSomeUselessRGBColor.green + fOffset.blue)
                / 3 + fBrightness;
                // ��һ�����ʽ����������, ���ǿ�������
                // һ���Ʊ�������������������б��ʽ��
                // ��ͬ���ȼ����㡣
    }
    
    
    for (int32 index = 0; index < count; index++) {
        DoSomething(index);
        DoSomethingElse(index);
    }
 
    // ���ڵ��е����������Բ�Ҫʹ�ô�����,ֱ�Ӱ���������һ�м��ɡ�
    // (���Ƕ��ڶ�����䣬���������ǰ����ڴ�����֮��)
 
    if (condition)
        DoOneThingOnly(index);
     
    for (int32 index = 0; index < count; index++)
        DoOneThingOnly(index);
 
    // switch ��������ʽ
     
    switch (condition) {
        case label1:
            DoSomething();
            break;
    
        case label2:
        {
            // ����������� count �����������Ҫʹ��һ��
            // �����š�
            int32 count;
            ...
            DoSomething();
            break;
        }
    }
     
    ...
    CallingSomeFunction(firstArgument * 2 + someMoreStuff,
            secondArgument, thirdArgument);
            // ���ڽϳ��Ĳ���������һ����Ҫʹ��������
    ...
    
    const rgb_color kNeonBlue = {10, 10, 50, 255};

## ����

* ÿһ�в��ܹ����� 80 �У�����������һ��ʱ��ͨ��Ҫ����һ��������Ʊ������������������Щ��������ݡ�
* �ں���֮��ճ������оࡣ
* ��ÿ���ļ���β����һ�����з���

## ��ʶ��

* ʹ�ü������ı�ʶ��������ʹ������ r��aMessage��theView��MyDraw(who's draw?) �ȱ�ʶ����ͬʱ��Ҫ����ʹ�óɶԵı�ʶ�������磺ProcessMessage �� DoProcessMessage��AddTasks1 �� AddTask����Haiku�������Ƽ�ʹ������ rect��message��invokeMessage, view��targetView��DrawBorder��ProcessMessage �� ProcessMessageinternals ����ProcessMessageDetails �ȵı�ʶ����
* �������ṹ�������������������ռ�ͺ�����Ӧ���Դ�Щ�ַ���ͷ�������������м�Ҫʹ��һЩ��д�ַ����������������˿��Ը��õ������룬�����벻Ҫʹ���»��ߣ���
* ������Ҫ��Сд�ַ���ͷ��ͬ�����������м�Ҫʹ��һЩ��д�ַ���
* ��Ա������Ҫ�� ��f�� �ַ���ͷ����ʽ���£�

    `int32 fMemberVariable;`
* ������Ҫ�� ��k�� �ַ���ͷ����ʽ���£�

    `const uint32 kOpenFile = 'open';`

    ����Ҫע����ǣ����ֳ�����ʽ�ͱ�׼�� Be API �������ǲ�ͬ�ģ�

* ȫ�ֱ�����Ҫ���� ��g�� ǰ׺�ַ�����̬������ͬ�������һ��������Ҫ���� ��s�� ǰ׺�ַ���
* ˽�еķ�����Ҫ����һ�� ��_�� ǰ׺��

## ��������

* �ڱ����ı������÷�Χ����������Ҫ��������еı����������ں�����ͷ���������������������� C ���������������������������ĺô��ǿ��Ժ����׼������Ƿ񱻺���Ľ����˳�ʼ�������Ҵ���ο��Ժ����׵ı�����ճ���������ĵط���
* ����ʹ���ܹ���ӳ������������֣�ͬʱ����Ϊ�˲�ͬ��Ŀ�Ķ��ظ�ʹ�õ�����ʱ������
* ����ʹ��ȫ��������ʹ����д�����磺ʹ�� msg ������ message���ᳫʹ�� rect��frame��bounds �������r��ʹ�� menuItem ���� mi��

## ʹ�� Haiku ���õ� API�����͵�

* �Ƽ�ʹ�� Haiku API ���ܵ��ã��������Լ�������ơ�
* �Ƽ�ʹ�� BobjestList ������ Blist��BobjectList �ṩ�˰�ȫ���ͣ���ѡ����ĿȨ�ޣ��������������õ���ƣ����ӵ�ģ��ʵ�����������Ӵ���ĳ��ȡ�
* �ڽ����ַ�������ʱ���Ƽ�ʹ�� Bstring ����� malloc��strdup��free �ȡ�
* �Ƽ�ʹ�� Bstring �е� << �����������̶���С�Ļ������� sprintf ������
* �Ƽ�ʹ���� SupportDefs.h �ж�������� int32��unit32 �������� int��long �ȡ�ʹ�� status_t ������ int��int32 ���Է��س�����Ϣ���ں��ʵĵط�ʹ�� off_t ������ int64��

## ע��

* ���ʵ��ĵط��Դ������ע�͡�
* �Ƽ�ʹ�� C++ ����ע�͡�
* ע��Ҫ�ʵ�������̫�����ࡣ�����漴��ע�͹�������ӣ���

        ...
        index++; //index ����������
        ...
             
        ...
        /* InitProgress
         *
         */
        void
        InitProgress(int param1)
        {
        ...

* �Ƽ�����Ҫע�͵Ĵ�����ǰ�������ĵط�����ע�ͣ�����Ҫ��һ������ν���ע��ʱ���ڴ����ǰ�����ע�ͣ�����Ҫ��ĳһ�д������ע��ʱ����������һ������һ���Ʊ�����ٽ���ע�ͣ���

        // ����վ������Ҫ��ʾ�����ѹ��ؾ�Ļ���վ��
        // ���ļ�
        // (��һ�������������ע��)
        BVolumeRoster volRoster;
        volRoster.Rewind();
        BVolume volume;
        while (volRoster.GetNextVolume(&volume) == B_OK) {
            if (!volume.IsPersistent())
                continue;
            ...
            ...
            BPoseView::WatchNewNode(&itemNode,watchMask,lock.Target());
                // ����ѽڵ���ʾ����ʱ��֮ǰ��������Ϊ 
                // Model ���Ỻ���ļ����ͺ�ƫ�õĳ���
                // (����һ�д���������ע��)

* ���������ڴ����к����ע�ͣ��⽫�������಻�㣨ͨ���Ƽ��ڶ����н���ע�ͣ���

        ...
        if (this < is && a < very && long != condition) { // ...
        ...
        if (this < is && a < very && long != condition) {
            // ��ע���ǹ�������ĳ��������ģ�
            // ���������ע�ͽ����������⡣
        ...

* ��Ҫ��������ֻ���������д��ӵ�ע���У�������ύ�Ĳ����������ˣ�������ֽ�������� GIT ��ǩ����־�С����е��˶�����ͨ�����ַ�ʽ��ʶ����Ĵ��롣
* ������ע��������Լ������飬��Ҫ�ڴ����а����������Ƶ�����:

    `// this is a hack!`
    
    �෴�Ľ���Ϊʲô����Ϊֵ������Ĵ����� hack��

    `// ����Ĵ�����û���õģ������ܹ��ܺõĴ������������`

## ��ɺͰ�Ȩ

* �Ƽ���Դ�ļ�����ɺͰ�Ȩ������ʽ���£�

        /*
         * Copyright 2004-2007 Haiku Inc. All rights reserved.
         * Distributed under the terms of the MIT License.
         *
         * Authors:
         *		Jonathan Smith, optional@email
         *		Developer Name, optional@email
         */

    �������е��ļ����� ��Haiku Inc.�� ��Ȩ�����������������ߵ����ֶ������ϰ��ַ��Ⱥ�����

* ����ͷ�ļ���Ӧ���� ��Haiku Inc.�� ��Ȩ���������Ҳ����г�����������
* ������ϣ���Լ���������Ȩ������ļ����ײ�Ӧ��������ʾ�������������ֵ����򣬺�ʾ���е����ƣ�

         /*
         * Copyright 2007 Jane Doe, optional@email
         * Copyright 2003-2005 Some Developer, optional@email
         * All rights reserved. Distributed under the terms of the MIT License.
         */

* ��һЩ��������£�����ܱ���������ļ��İ�Ȩ�б������չ�������Ϊ ��Haiku Inc.�� ���һ�а�Ȩ���������Ҳ���ʾ��һ���������ֽ������򣬶����ǽ����Ӹոյ����Ӻ��Ƽ���������ѡһ�֡�
* ͷ�ļ��еİ�Ȩ�����У������֤��ͷ������֮��û�п��С�
* �ڰ�Ȩ����ͷ��֮�󣨰���ͷ�ļ��е�ͷ������������������֮ǰ���������������С�

## ���ô���͵��Դ���

* ������޷���֤�Լ����׵Ĵ�����������벻Ҫ�������õģ�������Ļ���ʹ�� #if 0��ed �Ĵ��롣��ĸ���������Ҫ�кܸߵ������������ܹ��ܺõش�����Ҫ����Ĵ��롣�������ĳЩԵ�ʣ���ϣ���ջ���Ĵ�����߽��������������ο��������ʹ��Դ������ƹ���
* ��Ҫ���¼򵥵ı�ע�͵��ĵ���������롣���� BeDebug ����˴󲿷ִ���ĵ�������Ĺ���������ͨ�����öϵ������Դ��룬�� #if DEBUG ������а�����Ҫ���ԵĴ��롣һ��Ҫ��֤���ԵĴ����ڱ���ʱû�о��棬��������ȷ�ԣ���֤��ĸ��Ĳ����ƻ��Ѿ����Թ��Ĵ��룬���򣬾ͻ���Ҫ����Щ��������޸ģ�����Ҳ�������� Debug.h �е��������������д�����ԡ�

## ������Ҫ��

* ʹ�����͵������dynamic_cast��static_cast��const_cast��reinterpret_cast �ȵȣ��������ʱ�������
* ѡ�ú��ʵĳ�����
* ������ܵĻ�������ѡ��ʹ�� stack-allocated ���������� heap-allocated ����
* ʹ���Զ���(AutoLock)����������е�������������Դ��ȡ����Ҫֱ��ʹ�� Blocker �е� Lock() �� Unlock()����ʹ���Զ��Զ���ʱ,�Ƽ�ʹ�� Autolock ģ�����Ҫʹ�� BAutolock��
* ������ʱ����Ҫ��if����н��и�ֵ��

        if ((err = entry.GetRef(&ref)) == B_OK)
        ...

* ������ while ѭ�����������н��и�ֵ�����磺

        BMenuItem* item;
        int32 index = 0;
        while ((item = ItemAt(index++)) != NULL) {
            ...
    
    �෴���� for ������������У��������߳����Ƿǳ���Ч�ʡ�
    
        for (int32 index = 0; ; index++) {
            BMenuItem* item = ItemAt(index);
            if (item == NULL)
                break;
            ...

* ��ɾ�������ͷŶ���֮ǰ����Ҫ�� NULL ��顣
        
        // ����ʾ��
        if (fIcon)
            delete fIcon;
        
        // ����ʾ��
        if (fIconBuffer)
            free(fIconBuffer);

* ��Ҫ�ڷ���ֵ�������Բ���ţ�

        // ����ʾ��
        return (fList.ItemAt(index));

* �ڼ��λ����ʱ��Ҫʹ��Բ���ţ�һ��ĸ�ʽ���£�
        if ((a & 3) != 0 && (b & 4) == 0)
            ...
 
        // ���� - C/C++ ����������ȼ���ͬ
        if (a & 3 && xyz)
            ...

* ������������������
* �ڹ��캯���ڲ�ʹ�ó�ʼ���б��������ʼ����Ա��
* ʹ�����õ� false/true ������ FALSE/TRUE #define��
* ������ĸ˳��� #include �������򣬶���Ҫ�� ��includes�� �� <includes> �ֿ�����
* һ��Դ�ļ���ͷ�ļ�Ӧ�ñ������ڸ��ļ����Ա�֤���ܹ�ʵ�������ı��룬����������ͷ�ļ�������õģ�POSIX���������ģ�����Ŀ¼�еģ�������ѭһ���Ĺ涨��Ϊ�˶���������ĸ�������������������API���з�����У�ʾ�����£����ǲ�Ҫ����ص�ע�ͣ���
        #include "ThisClass.h"
         
        // POSIX API headers
        #include <stdio.h>
        #include <string.h>
         
        // Haiku API headers
        #include <File.h>
        #include <OS.h>
         
        #include <PrivateHeader.h>
 
        #include "OtherLocalHeaders.h"

* ���û�б�Ҫ���벻Ҫʹ�� <·����/include. h >��������< sys/stat.h >����
* ָ���ʼ��ʱʹ�� NULL ������ 0��
* ����ʹ�� goto��
* ��Ҫʹ�ù��캯�����õ��﷨����ʼ��ָ�룬���磬���� NULL��

        // ����ʾ��
        BView* view(NULL);

    ʹ�ø��ӳ���ĸ�ֵ��ʽ��

        BView* view = NULL;

    �������ʵ���ʹ�ýṹ���������ö�ջ����ʹ�����������벻Ҫ�Դ˸е��ɻ󣩡�

* �ڱȽϺ������õĽ��/������ĳһ�������Ĵ�Сʱ����Ҫ�ѳ������ڱȽϱ��ʽ����ࣺ

        if (B_OK == file.InitCheck()) // ��Ҫʹ�����ָ�ʽ
            ...

    ����Աʹ����һ��ʽ��ȷ������ʹ���˱Ƚ϶����Ǹ�ֵ�����ֱ�ʾ�������ܲ��Ǻܳ���������Ҫ�ĺ�������/���÷�����Բ��Ǻ���Ҫ���Ҳࡣ��Ȼ Haiku ��ʹ�����ֱ�ʾ������һ������ĸ�ֵ�����ɸ����ľ���������֡�

* �Ƽ�ʹ�� C ��ʽ��ͷ�ļ���string.h��stdlib.h���������� C++ �µĶԵ��ļ���cstring��cstdlib����Haiku ��ͷ�ļ�ͨ���� C++ �����ܵģ�����Ҫʹ�����ֱ�ͨ��ʩ

## ����鹤��

��������Ĺ�����Ȼ����һЩ���⣬��ʱ����ܻᷴ������Ľ�������������ڴ����ʱ�仹���ܹ�����ʹ�ã����ҿ����ҳ�һЩ���������⡣��Щ������Ȼ���ڿ���֮�У���ӭ�����һЩ�д���Ľ��顣

* Checkstyle.py ��һ�������� Python ������ Haiku Դ������ src/tools/checkstyle/ �¿����ҵ���ص�Դ�룩���������Ա�׼����ķ�ʽ����������ص����⣬��������һ�������Ķ����и�������ʾ�� HTML ���档ʾ�����£�

        python src/tools/checkstyle/checkstyle.py src/apps/deskcalc

    �������㴦��HaikuԴ�����Ķ���Ŀ¼���⽫�ᷴ����� src/apps/deskcalc Ŀ¼�������ʹ�� --help ѡ�����˽�����ʹ�÷�����

* [HaikuCodingGuidelinesVIM](http://dev.haiku-os.org/wiki/HaikuCodingGuidelinesVIM) ��һ�� Vim �ı��༭��������һ��Ľű���