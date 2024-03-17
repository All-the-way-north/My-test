# **IntLanguageDataAbstract** 测试编写



## 具体实现



### 第一层测试

#### 浅层测试初始化字符串

```c++
TEST(FIRST_SHALLOW, INI_COM) {
		IntLanguageDataAbstract* test;
		test = new IntLanguageDataXML();

		auto result = test->iniCom();

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "初始化失败";
		result = test->desCom();

		delete test;

	}
```

此测试首先创建一个抽象基类对象 test并且对它初始化，将初始化的结果返回来编写断言，返回的运行信息是存储成功则通过断言，否则打印相关初始化失败的信息。

#### 浅层测试销毁字符串

```c++
TEST(FIRST_SHALLOW, DES_COM)
	{
		IntLanguageDataAbstract* test;
		test = new IntLanguageDataXML();

		auto result = test->iniCom();
		result = test->desCom();

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "未成功销毁";

		
		delete test;

	}
```

测试首先创建一个抽象基类对象 test并且对它初始化,然后执行销毁内存中的字符串并返回运行信息，断言则是由返回的信息来判断程序是否成功销毁了字符串。

#### 浅层测试搜索字符串

```c++
TEST(FIRST_SHALLOW, SEARCH_STRING)
	{
		IntLanguageDataAbstract* test;
		test = new IntLanguageDataXML();

		auto result = test->iniCom();

		IntLanguageDataAbstract::String s;
		result = test->searchString("1","CHN", s);

		EXPECT_NE(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "请检查是否存在内存未清理的异常情况";

		result = test->storeString("1", "CHN", "CBD");

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "将字符串存储到文件中失败";

		result = test->searchString("1", "CHN", s);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "请检查是否存在内存未清理的异常情况";

		result = test->desCom();

		delete test;

	}
```

此测试在初始化类对象test后，由第一个断言``EXPECT_NE(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS)``判断程序能否在未存入字符串的情况下搜索字符串，期望不能成功搜索，返回结果与期望一致。

在通过id为1，language为CHN向文件中存入字符串”CBD“后，为其返回信息编写断言判断是否成功存入字符串，期望返回SUCCESS，返回结果与期望一致。此时再次通过id为1，language为CHN查询字符串便可返回SUCCESS，结果与期望一致，断言通过。

#### 浅层测试存储字符串

```c++
TEST(FIRST_SHALLOW, STORE_STRING)
	{
		IntLanguageDataAbstract* test;
		test = new IntLanguageDataXML();
		auto result = test->iniCom();

		IntLanguageDataAbstract::String s = "ABABAB";
		 result = test->storeString("1","CHN", s);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "将字符串存储到文件中失败";
		
		result = test->desCom();

		delete test;

	}
```

此测试在初始化类对象test后通过id为1，language为CHN向文件中存入字符串”ABABAB“，由程序执行的返回结果编写断言，期望能够成功存储，返回结果与期望一致。

#### 浅层测试修改字符串

```c++
TEST(FIRST_SHALLOW, CHANGE_STRING)
	{
		IntLanguageDataAbstract* test;
		test = new IntLanguageDataXML();
		auto result = test->iniCom();

		IntLanguageDataAbstract::String s  = "BCS";
		result = test->changeString("1","CHN", s);

		EXPECT_NE(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "在文件中更改字符串失败";

		result = test->storeString("1", "CHN", "CBD");

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "将字符串存储到文件中失败";

		result = test->changeString("1", "CHN", s);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "在文件中更改字符串失败";

		EXPECT_EQ(s, "BCS") << "在文件中更改字符串失败";

		result = test->desCom();

		delete test;

	}
```

此测试在初始化类对象test后，在未存储字符串的情况下修改id为1，language为CHN对应的字符串，期望不会返回SUCCESS，实际返回结果是CHANGE_STRING_ERROR_ID ，断言通过。

当通过id为1，language为CHN向文件中存入字符串”CBD“之后，再次执行修改字符串，将字符串更改为”BCS“，期望返回SUCCESS，程序返回与期望一致，断言通过。

#### 浅层测试删除字符串

```c++
TEST(FIRST_SHALLOW, DEL_STRING)
	{
		IntLanguageDataAbstract* test;
		test = new IntLanguageDataXML();
		auto result = test->iniCom();

		result = test->delString("1","CHN");

		EXPECT_NE(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "未存储的字符串应当不能被删除";

		result = test->desCom();

		delete test;

	}
```

此测试在初始化类对象test后，由断言``EXPECT_NE(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS)``判断程序能否在未存入字符串的情况下删除字符串，期望不能成功搜索，返回结果与期望一致。

#### 测试是否将所有国际化语言的字符串存入文件

```c++
TEST(FIRST_SHALLOW, GET_MAP1)
	{
		IntLanguageDataAbstract* test;
		test = new IntLanguageDataXML();
		auto result = test->iniCom();

		IntLanguageDaaAbstract::IntLanMap* intLanMap=new IntLanguageDataAbstract::IntLanMap();
		result = test->getMap(intLanMap);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "将所有的国际化语言字符串存放到intLanMap失败";

		result = test->desCom();

		delete test;

	}
```



#### 浅层测试清空文件中所有数据

```c++
TEST(FIRST_SHALLOW, CLEAR_DATA)
	{
		IntLanguageDataAbstract* test;
		test = new IntLanguageDataXML();
		auto result = test->iniCom();

		result = test->clearData();

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "清除所有数据失败";

		result = test->desCom();


		delete test;

	}
```

断言``EXPECT_NE(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS)``判断程序能否删除文件中所有字符串，期望返回SUCCESS，返回值与期望一致。

***



### 第二层测试

#### 深层测试搜索字符串

```c++
TEST(SECOND_DEEP,SEARCH_STRING )
	{
				IntLanguageDataAbstract* test;
		test = new IntLanguageDataXML();
		auto result = test->iniCom();

		IntLanguageDataAbstract::String  s,s1,s2;

		result = test->searchString(" ", " ", s);

		EXPECT_NE(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "请查看id和language都为空是存储的字符串";

		result = test->searchString("1", "CHN", s);

		EXPECT_NE(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "请检查是否存在未清空内存数据的异常情况";

		s = "BCD";
		result = test->storeString("1", "CHN", s);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "存储字符串失败";

		result = test->searchString("1", "CHN", s1);
		
		EXPECT_EQ(s, s1) << "查找字符串失败";
    	
    	result = test->storeString("CHN", "1", s2);

		EXPECT_NE(s, s2) << "查找字符串错误";

		result = test->searchString("2", "CHN", s2);

		EXPECT_NE(s, s2) << "查找字符串错误";

		result = test->searchString("1", "chn", s2);

		EXPECT_NE(s, s2) << "查找字符串错误";

		result = test->searchString("2", "chn", s2);

		EXPECT_NE(s, s2) << "查找字符串错误";

		result = test->clearData();
		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "清空所有数据失败";

		result = test->searchString("1", "CHN", s2);
		EXPECT_NE(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "清空数据有误或者内存错误";

		result = test->desCom();

		delete test;

	}
```

此测试首先创建一个抽象基类对象 test并且对它初始化，第一个断言测试在id和language都为空的条件下查询字符串的结果，期望结果不是SUCCESS即不能查询成功，程序返回执行信息是SEARCH_STRING_ERROR_ID，符合预期结果，断言成功。

第二个断言判别程序能否在未存入字符串的条件下查询到字符串，期望结果不是SUCCESS，返回结果是SEARCH_STRING_ERROR_ID，符合预期结果，断言成功。

第三个断言判断能否通过id为1，language为CHN向文件中存入字符串"BCD"，期望返回SUCCESS，实际返回SUCCESS，断言成功。

第四个断言则是由id为1，language为CHN 在文件中查询对应字符串，期望返回SUCCESS，实际返回SUCCESS，，说明存储成功，搜索字符串也通过，断言成功。

接下来的断言则是更换id与language的顺序，改变language的大小写以及更改id来判断是否能正确从文件中读取字符串，期望结果不是SUCCESS，实际返回符合预期结果，断言成功。



### 深层测试存储字符串

```c++
TEST(SECOND_DEEP, STORE_STRING)
	{
		IntLanguageDataAbstract* test;
		test = new IntLanguageDataXML();
		auto result = test->iniCom();

		IntLanguageDataAbstract::String s , s1;

		result = test->searchString("1", "CHN", s);

		EXPECT_NE(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "请检查是否存在未清空内存的异常情况";

		s = "CBD";
		result = test->storeString("1","CHN", s);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "将字符串存储到文件中失败";

		result = test->searchString("1", "CHN", s1);

		EXPECT_EQ(s1, s) << "查找字符串失败或者存储字符串失败";
		//EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "将字符串存储到文件中失败";
		
		 result = test->clearData();
		 EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "未能清空内存所有数据";
	
		 result = test->desCom();
    
		 delete test;
	}
```

此测试首先创建一个抽象基类对象 test并且对它初始化，第一个断言测试在id和language都为空的条件下查询字符串的结果，期望结果不是SUCCESS即不能查询成功，程序返回执行信息是SEARCH_STRING_ERROR_ID，符合预期结果，断言成功。

第二个断言然后通过id、 language在文件中存储字符串s中，并返回运行信息。期望存储字符串返回SUCCESS，实际返回与期望一致，断言成功。

然后再次通过id、 language在文件中查询字符串并存储在s1中，断言判断搜索的字符串是否与存入的字符串相等，期望相等，实际与期望一致，断言成功。

#### 深层测试修改字符串

```c++
TEST(FIRST_DEEP, CHANGE_STRING)
	{
		IntLanguageDataAbstract* test;
		test = new IntLanguageDataXML();
		auto result = test->iniCom();

		IntLanguageDataAbstract::String s1="CBD",s2 = "abbb",s3;
		result = test->storeString("1", "CHN", s1);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "将字符串存储到文件中失败";

		result = test->searchString("1", "CHN", s3);

		EXPECT_EQ(s1,s3) <<"在文件中查找字符串失败";

		result = test->changeString("1","CHN", s2);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "在文件中更改字符串失败";

		result = test->searchString("1", "CHN", s3);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "在文件中查找字符串失败";
		EXPECT_EQ(s2,s3) << "字符串修改失败";

		test->clearData();

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "未能清空内存所有数据";

		result = test->desCom();

		delete test;

	}
```

测试首先创建一个抽象基类对象 test并且对它初始化后，编写第一个断言判断是否通过id和language向文件中成功存储字符串，期望与返回结果均为SUCCESS，断言成功。

第二个断言判断能否查找到相关字符串，返回结果与期望一致，说明确实存储成功。

第三个断言判断通过id、language将字符串更改为s2是否成功，期望返回SUCCESS，实际返回与期望一致，断言成功。

第四个断言继续查询对应字符串，将查询结果存放于s3中，期望s3与s2相等，期望与实际一致，断言成功，说明文件中的字符串确实被修改成功。

#### 深层测试删除字符串

```c++
TEST(SECOND_DEEP, DEL_STRING)
	{
		IntLanguageDataAbstract* test;
		test = new IntLanguageDataXML();
		auto result = test->iniCom();

		IntLanguageDataAbstract::String s = "CBDA",s1,s2;
		result = test->storeString("1", "CHN", s);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "将字符串存储到文件中失败";

		result = test->searchString("1", "CHN", s1);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "查找字符串模块失败";
		EXPECT_EQ(s, s1) << "查找字符串失败";

		result = test->delString("1","CHN");

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "在文件中删除字符串失败";

		result = test->searchString("1", "CHN", s2);

		EXPECT_NE(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "删除模块失败";
		EXPECT_NE(s,s2) << "字符串删除失败";

		result =test->clearData();

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "未能清空内存所有数据";
	
		result = test->desCom();

		delete test;
	}
```

测试首先创建一个抽象基类对象 test并且对它初始化，然后通过ID、 language在文件中存储字符串s中，第一个断言判断字符串是否成功存入文件，预期和期望一致，断言成功。

第二个断言判断通过ID、 language在文件中查询字符串并存储在s1中是否成功，预期和期望一致，断言成功。

第三个断言则是判断查询到的字符串与存入的字符串是否一致，期望一致，实际也一致，说明查询与存储都成功。

接下来通过ID、 language在文件中删除对应字符串，期望成功删除返回SUCCESS，期望与实际一致，断言成功。

执行了删除字符串模块后，再次通过ID、language查找相关字符串，期望不会返回SUCCESS，并且查询的结果也与原字符串不相等，期望与实际返回结果一致，断言成功。

#### 深层测试清除文件所有数据

```c++
TEST(FIRST_DEEP, CLEAR_DATA)
	{
		IntLanguageDataAbstract* test;
		test = new IntLanguageDataXML();
		auto result = test->iniCom();

		IntLanguageDataAbstract::String s{"abbb"},s1,s2;
		result = test->storeString("1", "CHN", s);
		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "存储字符串失败";

		result = test->searchString("1", "CHN", s1);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "查找字符串失败";
		EXPECT_EQ(s, s1) << "查找字符串失败";

		result = test->storeString("2", "UK", "iooooo");

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "存储字符串失败";

		result = test->searchString("2", "UK", s1);

		EXPECT_EQ("iooooo", s1) << "查找字符串失败";
		
		result = test->clearData();
		
		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "清除所有数据失败";

		result = test->searchString("1", "CHN", s2);

		EXPECT_NE(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "清除所有数据失败，任然能搜索相关字符串";
		
		EXPECT_NE(s, s2) << "清除数据删除失败";

		result = test->clearData();

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "未能清空内存所有数据";

		result = test->desCom();


		delete test;

	}
```

测试初始化类对象之后，通过两个不同的id、language存入不同的两段字符串，断言判断是否存储成功以及是否能查询到对应字符串，期望与返回结果一致，断言成功。

接下来执行清空内存中所有字符串的指令，断言判断返回结果是否为SUCCESS，返回与期望一致。、

再次通过上述id、language查询字符串不能查询成功，说明内存中确实没有对应的字符串存储，断言成功。

***



### 第三层测试

#### 深层测试搜索字符串

```c++
TEST(SECOND_DEEP,SEARCH_STRING )
	{
		IntLanguageDataAbstract* test;
		test = new IntLanguageDataXML();
		auto result = test->iniCom();

		IntLanguageDataAbstract::String s, s1 = "CBD", s2 = "abbb";
		result = test->searchString("1", "CHN", s);

		EXPECT_NE(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "请检查是否有未清空内存的异常情况";

		result = test->storeString("1", "CHN", s1);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "字符串s1存储失败";

		result = test->searchString("1", "CHN", s);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "字符串查找失败";

		EXPECT_EQ(s, s1) << "字符串查找错误";

		result = test->searchString("1", " ", s);

		EXPECT_NE(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "可以通过提供的id和空的language查询字符串";

		result = test->searchString(" ", "CHN", s);

		EXPECT_NE(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "可以通过空的ID与一个被提供的language查询字符串";

		result = test->storeString("1", "CHN", s2);

		EXPECT_NE(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "字符串s2存储失败";

		result = test->searchString("1", "CHN", s);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "字符串查找失败";

		EXPECT_EQ(s, s1) << "是否选择修改字符串？";

		result = test->clearData();

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "清空所有数据失败";

		result = test->desCom();

		delete test;

	}
```

测试在第二层测试的基础上增添id不为空，language为空是搜索字符串的查询结果，期望返回值不是SUCCESS，实际返回SEARCH_STRING_ERROR_LANGUAGE，与期望一致，说明不能在id不为空，language为空的条件下查询字符串。

下一个断言则是判断能否在在id不空，language不为空的条件下查询字符串，期望返回值不是SUCCESS，实际返回SEARCH_STRING_ERROR_ID，与期望一致，说明不能在id为空，language不为空的条件下查询字符串。

测试编写了一段通过同一个id和language向文件中存入不同的字符串，期望能存储不同的字符串，实际返回结果是STORE_STRING_ERROR_LANGUAGE，即不能成功向文件中存入,实际与期望一致，断言成功。

再次查找字符串，所得字符串应当与第一次存入的字符串一致，否则返回错误。

#### 深层测试存储字符串

```c++
TEST(SECOND_DEEP, STORE_STRING)
	{
		IntLanguageDataAbstract* test;
		test = new IntLanguageDataXML();
		auto result = test->iniCom();

		IntLanguageDataAbstract::String s,s1 = "CBD", s2 = "abbb";
		result = test->storeString("1", "CHN", s1);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "存储字符串s1失败";

		result = test->searchString("1", "CHN", s);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "查询字符串失败";
		EXPECT_EQ(s,s1) << "请检查是否查询的字符串有误";

		result = test->storeString("1", "CHN", s2);

		EXPECT_NE(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "存储字符串s2失败";

		result = test->searchString("1", "CHN", s);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "查询字符串失败";

		EXPECT_EQ(s, s1) << "请查看字符串是否被错误修改";

		result = test->storeString(" ", " ", s1);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "不能为空id和language存入字符串";

		result = test->clearData();

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "清空所有数据失败";
	

		result = test->desCom();

		delete test;

	}
```



测试在第二层测试的基础上添加通过同一个id、language向文件中存入不同的字符串，期望能存储不同的字符串，实际返回结果是STORE_STRING_ERROR_LANGUAGE，即不能成功向文件中存入，实际返回结果与期望一致，断言成功。

测试编写了通过空的id、空的language向文件中存储字符串，期望返回结果不是SUCCESS，实际返回结果是STORE_STRING_ERROR_LANGUAGE，即不能成功向文件中存入，与预期一致，断言成功。

#### 深层测试修改字符串

```c++
TEST(SECOND_DEEP, CHANGE_STRING)
	{
		IntLanguageDataAbstract* test;
		test = new IntLanguageDataXML();
		auto result = test->iniCom();

		IntLanguageDataAbstract::String s, s1{ "CBD" }, s2 = "abbb", s3;
		result = test->storeString("1", "CHN", s1);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "将字符串存储到文件中失败";

		result = test->searchString("1", "CHN", s);

		EXPECT_EQ(s1, s) << "在文件中查找字符串失败";

		result = test->changeString("1", "CHN", s2);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "修改字符串失败";

		result = test->searchString("1", "CHN", s);

		EXPECT_NE(s1, s) << "修改字符串失败,仍然为原字符串";

		EXPECT_EQ(s2, s) << "修改字符串失败，没有更新为所给字符串";

		result = test->changeString("1", "chn", s2);

		EXPECT_NE(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "没有对应的id和language是否可以修改字符串";

		result = test->changeString(" ", " ", s2);

		EXPECT_NE(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "没有对应的id和language是否可以修改字符串";
	
		result = test->clearData();

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "清空所有数据失败";

		result = test->desCom();


		delete test;

	}
```



此测试在第二次测试的基础上，增添为id正确，language错误的参数修改字符串，期望不能修改成功，返回结果与期望一致，断言成功。

此外，测试添加了修改id为空、language为空的字符串，返回结果是CHANGE_STRING_ERROR_ID，预期不反悔SUCCESS，与期望一致，断言成功。

#### 深层测试删除字符串

```c++
TEST(SECOND_DEEP, DEL_STRING)
	{
		IntLanguageDataAbstract* test;
		test = new IntLanguageDataXML();
		auto result = test->iniCom();

		IntLanguageDataAbstract::String s , s1 = "CBD",s2 = "abbb",s3;
		result = test->storeString("1", "CHN", s1);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "将字符串s1存储到文件中失败";


		result = test->searchString("1", "CHN", s);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "查找字符串模块失败";
		EXPECT_EQ(s, s1) << "查找字符串失败";

		result = test->delString("1", "CHN");

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "在文件中删除字符串失败";

		result = test->searchString("1", "CHN", s3);

		EXPECT_NE(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "删除模块错误";
		EXPECT_NE("CBD", s3) << "字符串删除失败";

		result = test->storeString("1", "CHN", s1);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "将字符串s1存储到文件中失败";

		result = test->storeString("1", "CHN", s2);

		EXPECT_NE(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "将字符串s2存储到文件中失败";

		result = test->delString("1", "CHN");

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "在文件中删除字符串失败";

		result = test->searchString("1", "CHN", s);

		EXPECT_NE(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "未找到对应字符串";

		result = test->clearData();

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "未能清空内存所有数据";
	
		result = test->desCom();


		delete test;
	}
```

测试在第二层测试的基础上添加通过同一个id、language向文件中存入不同的字符串，期望能存储不同的字符串，实际返回结果是STORE_STRING_ERROR_LANGUAGE，即不能成功向文件中存入，实际返回结果与期望一致，断言成功。

删除字符串也是将第一次存入的字符串删除，返回结果与预期一致，断言成功。

***

### 综合调用各个功能

```c++
TEST(DEEP, SOME)
	{
		
		IntLanguageDataAbstract* test;
		test = new IntLanguageDataXML();
		auto result = test->iniCom();

		IntLanguageDataAbstract::String  s1 = "CBD", s2 = "abbb", s3,s4,s;

		result = test->storeString("1", "CHN", s1);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "将字符串s1存储到文件中失败";

		result = test->searchString("1", "CHN", s3);

		EXPECT_EQ(s3, s1) << "查找字符串s1失败";

		result = test->storeString("1", "CHN", s2);

		EXPECT_NE(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "将字符串s2存储到文件中失败";

		result = test->searchString("1", "CHN", s4);

		EXPECT_EQ(s3, s4) << "对同一个id和language读入不同字符串可以实现";

		result = test->delString("1", "CHN");

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "在文件中删除字符串失败";

		result = test->searchString("1", "CHN", s);

		EXPECT_NE(s, s1) << "删除的是先存入的字符串";

		result = test->storeString("1", "CHN", s1);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "将字符串s1存储到文件中失败";

		result = test->changeString("1", "CHN", "okkkk");

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "在文件中将字符串s1修改失败";

		result = test->searchString("1", "CHN", s);

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "在文件中搜索字符串失败";

		EXPECT_EQ(s, "okkkk")<<"未成功修改字符串";

		result = test->clearData();

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "未能清空内存所有数据";

		result = test->desCom();

		EXPECT_EQ(result, IntLanguageDataAbstract::ErrorInfo::SUCCESS) << "销毁工作失败";

		delete test;
	}
```

测试首先创建一个抽象基类对象 test并且对它初始化，然后通过id、 language在文件中存储字符串s1中，在断言判断存储成功后查找字符串，为返回的运行信息编写断言判断是否执行成功。

这里测试仍然用到通过同一个id、language来存储字符串是否能成功的测试用例，预期不能成功，返回结果与预期一致，断言正确。

此后对字符串修改、查询、删除操作均是对第一次存入的字符串操作，断言成功，测试通过。

