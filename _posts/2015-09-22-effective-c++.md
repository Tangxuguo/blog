---
layout: post
title: "总结:Effective c++"
tags: [C]
---

这两天又刷了侯捷的《Effective c++》

摘一个笔记[董的博客《Effective C++》读书笔记](http://dongxicheng.org/cpp/effective-cpp-part1/)

本书共分为9节55个条款，从多个角度介绍了C++的使用经验和应遵循的编程原则。
1. 让自己习惯c++
2. 构造/析构/赋值运算
3. 资源管理
4. 设计与声明
5. 实现
6. 继承与面向对象设计
7. 模板与泛型编程
8. 定制new和delete
9. 杂项讨论


###1. 让自己习惯C++（Accustoming your self to C++）

条款01: 视C++ 为一个语言联邦
	
	本条款提示读者，C++已经不是一门很单一的语言，而应该将之视为一个由相关语言组成的联邦。从语言形式上看，它是一个多重范型编程语言(multiparadigm programminglanguage) ，一个同时支持过程形式(procedural)、面向对象形式(object-oriented)、函数形式(functional) 、泛型形式(generic) 、元编程形式(metaprogramming )的语言，从语言种类上看，它由若干次语言组成，分别为：
	(1)	C。说到底C++ 仍是以C 为基础。区块(blocks) 、语句( statements) 、预处理器( preprocessor) 、内置数据类型(built-in data types) 、数组(arrays) 、指针(pointers) 等统统来自C。
	(2)	Object-Oriented C++。这部分也就是C with Classes 的: classes (包括构造函数和析构函数) ，封装( encapsulation) 、继承( inheritance) 、多态(polymorphism) 、virtual 函数(动态绑定) ……
	(3)	Template C++。这是C++ 的泛型编程(generic programming) 部分，也是大多数程序员经验最少的部分。Template 相关考虑与设计己经弥漫整个C++，实际上由于templates 威力强大，它们带来崭新的编程范型(programming paradigm) ，也就是所谓的templatemetaprogramming (TMP，模板元编程)
	(4)	STL。 STL 是个template 程序库，它是非常特殊的一个。它对容器(containers) 、迭代器(iterators) 、算法(algorithms) 以及函数对象(function objects) 的规约有极佳的紧密配合与协调。

条款02: 尽量以const, enum, inline替换#define

	本条款讨论了C语言中的#define在C++程序设计中的带来的问题并给出了替代方案。
	C语言中的宏定义#define只是进行简单的替换，对于程序调试，效率来说，会带来麻烦，在C++中，提倡使用const，enum和inline代替#define；然而，有了consts 、enums 和inlines，我们对预处理器(特别是#define) 的需求降低了，但并非完全消除。#include 仍然是必需品，而#ifdef/#ifndef 也继续扮演控制编译的重要角色。目前还不到预处理器全面引迫的时候。

条款03: 尽可能使用const

	本条款总结了Const的使用场景和使用它带来的好处。
	关键字canst 多才多艺。你可以用它在classes 外部修饰global 或namespace作用域中的常量，或修饰文件、函数、或区块作用域(block scope) 中被声明为static 的对象。你也可以用它修饰classes 内部的static 和non-static 成员变量。面对指针，你也可以指出指针自身、指针所指物，或两者都(或都不〉是const。你应该尽可能地使用const，这样降低程序错误，使程序易于理解。
	此外，一个编程技巧是：当const 和non-const 成员函数有着实质等价的实现时，令non-const 版本调用const 版本可避免代码重复：
	
		class TextBlock {
		 
		 public:
		 
		  const char& operator[] (std::size_t position) const {
		 
		  ……
		 
		  return text[position];
		 
		 }
		 
		 char& operator[] (std::size t position)  {
		 
		  return const_cast<char&>( static_cast<const TextBlock&>(*this) [position]);
		 
		  //将op[]返回值的const 转除为*this 加上cons, 调用const op[]
		 
		}
	
条款04: 确定对象被使用前已先被初始化

	本条款告诫程序员，在C++程序设计中，应该对所有对象初始化，以避免不必要的错误，同时，给出了高效初始化对象的方法和正确初始化对象的方法。
	（1）初始化构造函数最好使用成员初值列(member initialization list) ，而不要在构造函数本体内使用赋值操作(assignment) 。初值列出的成员变量，其排列次序应该和它们在class 中的声明次序相同。
	考虑一个用来表现通讯簿的class ，其构造函数如下:
	
		class PhoneNumber { ... };
		 
		class ABEntry { //ABEntry =“Address Book Entry"
		 
		 public:
		 
		  ABEntry(const std::string& name, const std::string& address , const std::list<PhoneNumber>& phones);
		 
		 private:
		 
		  std::string theName;
		 
		  std::string theAddress;
		 
		  std::list<PhoneNumber> thePhones;
		 
		  int numTimesConsulted;
		 
		};
		 
		ABEntry: :ABEntry(const std: :string& nane , const std: : string& address,
		 
		const std::list<PhoneNumber>& phones)
		 
		  theName = narne; //这些都是赋值(assignments) ,
		 
		  theAddress = address; //不是始化(initializations)。
		 
		  thePhones = phones;
		 
		  numTimesConsulted = 0;
		 
		  int num TimesConsulted;
		 
		}
	正确而又高效的初始化对象的方法是：
	
		ABEntry: :ABEntry(const std: :string& nane , const std: : string& address,
		 
		const std::list<PhoneNumber>& phones)
		 
		: theName(name),
		 
		theAddress(address), //这些都是初始化
		 
		thePhones(phones),
		 
		numTimesConsulted(0)
		 
		{} // 构造函数体是空的
	C++ 有着十分固定的”成员初始化次序”。次序总是相同： base class早于其derived classes 被初始化，而class 的成员变量总是以其声明次序被初始化。回头看看ABEntry. 其theName 成员永远最先被初始化，然后是theAddress，再来是thePhones，最后是numTimesConsulted。即使它们在成员初值列中以不同的次序出现(很不幸那是合法的)，也不会有任何影响。
	（2）C++ 对”定义于不同编译单元内的non-local static 对象”的初始化次序并无明确定义。为免
		class FileSystem { ... };
		 
		FileSystem& tfs() //代替tfs对象
		 
		{
		 
		  static FileSystem fs; // 以local static的方式定义和初始化object
		 
		  return fs; // 返回一个引用
		 
		}
		 
		class Directory { ... };
		 
		Directory::Directory( params )
		 
		{
		 
		  ...
		 
		  std::size_t disks = tfs().numDisks();
		 
		  ...
		 
		}
		 
		Directory& tempDir() // 代替tempDir对象，
		 
		{
		 
		  static Directory td;
		 
		  return td;
		 
		}
###2. 构造/析构/赋值运算（Constructors，Destructors，and Assignment Operators）
	条款05: 了解C++ 默默编写并调用哪些函数
	本条款告诉程序员，编译器自动为你做了哪些事情。
	用户定义一个empty class (空类)，当C++ 处理过它之后，如果你自己没声明，编译器就会为它声明(编译器版本的)一个copy 构造函数、一个copy assignment操作符和一个析构函数。此外如果你没有声明任何构造函数，编译器也会为你声明一个default 构造函数。所有这些函数都是public 且inline 。举例，如果你写下:
	1
	class Empty { };
	这就好像你写下这样的代码:
	
		class Empty {
		 
		 public:
		 
		  Empty() { ... }
		 
		  Empty(const Empty& rhs) { ... )
		 
		  -Empty( ) { ... }
		 
		  Empty& operator=(const Empty& rhs) { ... }
		 
		};
	需要注意的是，只要你显式地定义了一个构造函数（不管是有参构造函数还是无参构造函数），编译器将不再为你创建default构造函数。
	条款06: 若不想使用编译器自动生成的函数，就该明确拒绝
	本条款告诉程序员，如果某些对象是独一无二的（比如房子），你应该禁用copy 构造函数或copy assignment 操作符，可选的方案有两种：
	(1)	定义一个公共基类，让所有独一无二的对象继承它，具体如下：
	
		class Uncopyable {
		 
		 protected: //允许derived对象构造和析构
		 
		  Uncopyable () {}
		 
		  -Uncopyable(} { }
		 
		 private:
		 
		  Uncopyable(const Uncopyable&}; //但阻止copying
		 
		  Uncopyable& operator=(const Uncopyable&);
		 
		};
	为阻止HomeForSale对象被拷贝，唯一需要做的就是继承Uncopyable:
	
		class HomeForSale: private Uncopyable {
		 
		  …
		 
		};
	这种方法带来的问题是，可能造成多重继承，这回导致很多麻烦。
	(2)	创建一个宏，并将之放到每一个独一无二对象的private中，该宏为：
	
			// 禁止使用拷贝构造函数和 operator= 赋值操作的宏
			 
			// 应该类的 private: 中使用
			 
			#define DISALLOW_COPY_AND_ASSIGN(TypeName) \
			 
			TypeName(const TypeName&); \
			 
			void operator=(const TypeName&)
	这种方法比第一种方法好，google C++编程规范中提倡使用该方法。
	
条款07: 为多态基类声明virtual 析构函数

	本条款阐述了一个程序员易犯的可能导致内存泄漏的错误，总结了两个程序员应遵守的百编程原则：
	（1）polymorphic (带多态性质的) base classes 应该声明一个virtual 析构函数。如果
	class 带有任何virtual 函数，它就应该拥有一个virtual 析构函数。这样，但用户delete基类指针时，会自动调用派生类的析构函数（而不是只调用基类的析构函数）。
	（2）Classes 的设计目的如果不是作为base classes 使用，或不是为了具备多态性(polymorphically) ，就不该声明virtual 析构函数。这是因为，当用户将一个函数声明为virtual时，C++编译器会创建虚函数表以完成动态绑定功能，这将带来时间和空间上的花销。

条款08: 到让异常逃离析构函数

	（1）析构函数绝对不要吐出异常。如果一个被析构函数调用的函数可能抛出异常，析构函数应该捕捉任何异常，然后吞下它们(不传播)或结束程序。
	（2）如果客户需要对某个操作函数运行期间抛出的异常做出反应，那么class 应该提供一个普通函数(而非在析构函数中)执行该操作。

条款09: 绝不在构造和析构过程中调用virtual 函数

条款10: 令operator= 返回一个reference to *this

	本条款告诉程序员一个默认的法则：为了实现“连锁赋值“，应令operator= 返回一个reference to *this。

条款11: 在operator= 中处理”自我赋值”

	本条款讨论了几种编写复制构造函数的正确方法。给出的结论是：确保当对象自我赋值时operator= 有良好行为。其中技术包括比较”来源对象”和”目标对象”的地址、精心周到的语句顺序、以及 copy-and-swap。
	(1)	复制构造函数的一种编写方式如下：
	
		Widget& Widget::operator=(const Widget& rhs)
		 
		{
		 
		  if (this == &rhs) return *this; //判断是否为同一个对象，如果是自我复制，直接返回
		 
		  delete pb;
		 
		  pb = new Bitmap(*rhs.pb);
		 
		  return *this;
		 
		}
	这个版本存在异常方面的麻烦，即，如果”new Bitmap” 导致异常(不论是因为分配时内存不足或因为Bitmap 的copy构造函数抛出异常) , Widget 最终会持有一个指针指向被删除的Bitmap 。
	(2)	让operator= 具备”异常安全性”往往自动获得”自我赋值安全”的回报。因此愈来愈多人对”自我赋值”的处理态度是倾向不去管它，把焦点放在实现”异常安全性” (exception safety) 上，即：
	
		widget& Widget::operator=(const Widget& rhs)
		 
		{
		 
		  Bitmap* pOrig = pb;
		 
		  pb = new Bitmap(*rhs.pb);
		 
		  delete pOrig;
		 
		  return *this;
		 
		}
	如果”newBitmap” 抛出异常， pb (及其栖身的那个Widget) 保持原状。即使没有证同测试(identity test) ，这段代码还是能够处理自我赋值，但这种方法效率比较低。
	(3)	另外一种比较高效的方法是：
	
		class Widget {
		 
		  ……
		 
		  void swap(Widget& rhs); //交换*this 和rhs 的数据:详见条款29
		 
		  ……
		 
		};
		 
		Widget& Widget::operator=(Widget rhs) //rhs是被传对象的一份复件(副本),注意这里是pass by value.
		 
		{
		 
		  swap(rhs); //将*this 的数据和复件/副本的数据互换
		 
		  return *this;
		 
		}

条款12: 复制对象时勿忘其每一个成分

	本条款阐释了复制对象时容易犯的一些错误，给出的教训是：
	(1)	Copying 函数应该确保复制”对象内的所有成员变量”及”所有base class 成分”。
	(2)	不要尝试以某个copying 函数实现另一个copying 函数。应该将共同机能放进第三
	个函数中，并由两个coping 函数共同调。换句话说，如果你发现你的copy 构造函数和copy assignment 操作符有相近的代码，消除重复代码的做法是，建立一个新的成员函数给两者调用。这样的函数往往是private 而且常被命名为init。

###3. 资源管理（Resource Management）
条款13: 以对象管理资源
	本条款建议程序员使用对象管理资源（如申请的内存），给出的经验是：
	(1)	为防止资源泄漏，请使用RAII(“资源取得时机便是初始化时机” (Resource Acquisition Is Initialization; RAII))对象，它们在构造函数中获得资源并在析构函数中释放资源。
	(2)	两个常被使用的RAII classes 分别是trl: : shared_ptr 和auto_ptr。前者通常是较佳选择，因为其copy行为比较直观。若选择auto_ptr，复制动作会使它(被复制物)指向null。

条款14: 在资源管理类中小心 copying 行为

	本条款提醒程序员，使用资源管理类时需根据实际需要管理copying行为，常见的有：抑制copying、施行引用计数法。

条款15: 在资源管理类中提供对原始资源的访问

	(1)	APIs往往要求访问原始资源( raw resources) ，所以每一个RAII class 应该提供一个”取得其所管理之资源”的办法。
	(2)	对原始资源的访问可能经由显式转换或隐式转换。一般而言显式转换(如调用get()函数)比较安全，但隐式转换对客户比较方便。

条款16: 成对使用new 和delete 时要采取相同形式

	本条款给出了程序员在申请和释放资源时常犯的错误，给出的经验是：
	如果你在new 表达式中使用[]，必须在相应的delete表达式中也使用[]；如果你在new 表达式中不使用[]，一定不要在相应的delete表达式中使用[]。

条款17: 以独立语句将newed 对象置入智能指针

	本条款指出了一个使用智能指针时常犯的错误，避免该错误可以这样做：
	以独立语句将newed 对象存储于(置入)智能指针内。如果不这样做，一旦异常被抛出，有可能导致难以察觉的资源泄漏。举例：
	processWidget(std::trl::shared ptr<W工dget> (new Widget) , priority());
	在调用processWidget之前，编译器必须创建代码，做以下三件事：
	(1)	调用priority
	(2)	执行”new Widget”
	(3)	调用trl: : shared_ptr 构造函数
	不同的C++ 编译器执行这三条语句的顺序不一样，但对priority的调用可以排在第一或第二或第三执行。如果编译器选择以第二顺位执行且priority函数抛出了异常，则新创建的对象Widget将导致内存泄漏，解决方法如下：
	std::trl::shared_ptr<Widget> pw(new Widget); //在独立语句内以智能指针存储Widget对象
	processWidget(pw, priority()); //这个调用肯定不存在内存泄漏
###4. 设计与声明（Designs and Declarations）

条款18: 让接口容易被正确使用，不易被误用

条款19: 设计class 犹如设计type

条款20: 提倡以pass-by -reference-to-const 替换pass-by-value

	尽量以pass-by-reference-to- const 替换pass-by-value。 前者通常比较高效，并可避免
	切割问题(slicing problem)（所谓切割问题，是指派生类的对象传给基类类型的参数时，派生对象中的一些属性会被截断），需要注意的是，该规则并不适用于内置类型，以及STL 的迭代器和函数对象。对它们而言，pass-by-value 往往比较适当（实际上，STL中的迭代器和函数对象只支持值传递）。
	条款21: 必须返回对象时，别妄想返回其reference
	本条款告诫程序员：绝不要返回pointer 或reference指向一个local stack 对象，或返回reference 指向一个heap-allocated对象，或返回pointer 或reference指向一个local static 对象而有可能同时需要多个这样的对象。
	下面一一举例说明。
	(1)	如果返回pointer 或reference指向一个local stack 对象:
	
		const Rational& operator* (const Rational& lhs,const Rational& rhs) {
		 
		  Rational result(lhs.n * rhs.n, lhs.d * rhs.d); //警告!糟糕的代码!
		 
		  return result;
		 
		}
	解释：result是local对象，而local 对象在函数退出前被销毁，这导致返回值坠入”无定义行为”。
	(2)	返回reference 指向一个heap-allocated对象
	
		const Rational& operator* (const Rational& lhs,const Rational& rhs) {
		 
		  Rational* result = new Rational(lhs.n * rhs.n, lhs.d * rhs.d);
		 
		  return *result;
		 
		}
	这种方式很容易造成内存泄露，如：
	
		Rational w, x, y , z;
		 
		w = x * y * z; //与operator*(operator*(x， y) , z) 相同，内存泄露
	(3)	返回pointer 或reference指向一个local static
	
		const Rational& operator* (const Rational& lhs, const Rational& rhs) {
		 
		  static Rational result;
		 
		  result = ... ;
		 
		  return result;
		 
		}
		 
		if((a * b) == (c * d)) {
		 
		  //当乘积相等时，做适当的相应动作;
		 
		} else {
		 
		  //当乘积不等时，做适当的相应动作;
		 
		}
	这样做的问题是，(a * b) == (c * d)永远为true。

条款22: 将成员变量声明为private

条款23: 宁以non-member 、non-friend 替换member 函数

条款24 :若所有参数皆需类型转换，请为此采用non-member 函数

	当类的构造函数（未声明为explicit）中包含参数时，该参数类型的对象或者数可隐式转换为该对象。如果多个这样的对象之间进行加减乘除，且要让他们全部进行类型转换，需要定义non-member函数（如友元函数）。

条款25:考虑写出一个不抛异常的swap函数

###5. 实现（Implementations）


条款26 :尽可能延后变量定义式的出现时间

	本条款告诉程序员，如果你定义了一个变量且该类型带一个构造函数或析构函数，当程序到达该变量时，你要承受构造成本，而离开作用域时，你要承受析构成本。为了减少这个成本，最好尽可能延后变量定义式的出现时间。
	举例说明
	
		// 此函数太早定义了变量"encrypte"
	 
		string encryptPassword(const string& passwor)
		{
	 
		  string encrypte;
	 
		  if (password.length() < MINIMUM_PASSWORD_LENGTH){
	 
		   throw logic_error("Password is too short";
	 
		}
	 
		 //进行必要的操作，将口令的加密版本放进encrypted之;
	 
		  return encrypte;
	 
		}
	如果该函数抛出异常，变量encrypted便不会被使用。较好的做法是将变量”encrypted”的定义放到要用它的前一句或者能够给它初值实参。
	
条款27:尽量少做转型

	本条款论证了为什么要尽量少做类型转换，并告诉读者，如果必须要进行类型转换，有哪些注意事项。
	常见的有三种类型转换方式：
	(1)	C风格：(T)expression
	(2)	函数风格：T(expression)
	(3)	C++ style cast
	[1] const_cast<T>(expression) : 移除变量的const属性
	[2] dynamic_cast<T>(expression) : 安全向下转型，即：基类指针/引用到派生类指针/引用的转换。如果源和目标类型没有继承/被继承关系，编译器会报错；否则必须在代码里判断返回值是否为NULL来确认转换是否成功。
	[3] reinterpret_cast<T>(expression)：底层转换
	[4] static_cast<T>(expression)：强迫隐式转换，如，将non-const对象转换为const对象，将int转换为double类型。
	对于这几种类型转换，给出的建议是“
	(1)	如果可以，尽量避免转型，特别是在注重效率的代码中避免dynamic_cast
	(2)	宁可使用C++ style转型，不要使用旧式转型。前者容易识别出来，而且也比较有分门别类的指掌。

条款28 :避免返回handles指向对象内部成分

	本条款告诫程序员，不要在类方法中返回handles（包括references、指针、迭代器）指向对象内部成分，因为这很容易导致空悬、虚吊（dangling）的对象。

条款29 :为“异常安全“努力是值得的

条款30 :透彻了解inlining的里里外外

	Inline函数可免除函数调用成本，提高程序执行效率，但它也会带来负面影响：(1)增大目标代码的大小，有时候会非常庞大，需要动用虚存，这将大大降低程序执行速度。 (2) inline 函数无法随着程序库的升级而升级。换句话说如果f 是程序库内的一个inline 函数，客户将”f 函数本体”编进其程序中，一旦程序库设计者决定改变f ，所有用到f 的客户端程序都必须重新编译。总之，将大多数inlining 限制在小型、被频繁调用的函数身上才是最明智的选择（根据80-20经验准则，80%的时间花在20%的函数上）。

条款31: 将文件间的编译依存关系降至最低

	本条款介绍了降低文件间编译依存关系的几种方法。
	常见的方法有两种：Handle class和Interface class.
	(1)	Handle class. main class内含一个指针成员，指向其实现类。这般设计常被称为pimpl idiom (pimpl 是”pointer to implementation” 的缩写，这种class称为“Handle class”。
	
		#include "Person.h"
		 
		#include "PersonImpl.h" //我们也必须#include PersonImpl的class 定义式，否则无法调用其成员函数:注意， PersonImpl 有着和Person完全相同的成员函数，两者接口完全相同。
		 
		Person::Person(const std::string& name , const Date& birthday,
		 
		const Address& addr)
		 
		: pImpl( new PersonImpl(name, birthday,  addr))
		 
		{}
		 
		std::string Person;;name() const {
		 
		  return p Impl->name( );
		 
		}
	(2)	Interface class. 实际上就是抽象基类
###6. 继承与面向对象设计（Inheritance and Object-Oriented Design）
条款32: 确定你的public 继承塑模出is-a 关系

	本条款告诉读者一个非常基本的继承思想：”public 继承”意味is唱。适用于base classes 身上的每一件事情一定也适用于derived classes 身上，因为每一个derived class 对象也都是一个base class 对象，但反之不然。
	
条款33: 避免遮掩继承而来的名称

	(1)	derived classes 内的名称会遮掩base classes 内的名称。在public 继承下从来没有人希望如此。举例：
	
		class Base {
		 
		 private:
		 
		  int x;
		 
		 public:
		 
		  virtual void mfl() = 0;
		 
		  virtual void mfl(int);
		 
		  virtual void mf2();
		 
		  void mf3 ();
		 
		  void mf3(double);
		 
		};
		 
		class Derived: public Base {
		 
		 public:
		 
		  virtual void mfl();
		 
		  void mf3 ();
		 
		  void mf4 ();
		 
		  …
		 
		};
	base class 内所有名为mfl 和mf3的函数都被derived class 内的mfl 和mf3函数遮掩掉了。从名称查找观点来看，Base: :mfl 和Base: :mf3 不再被Derived继承!
	(2)	为了让被遮掩的名称再见天日，可使用using 声明式或转变函数( forwarding
	functions) 。举例说明：
	[1] 使用using声明式
	
		class Base {
		 
		 private:
		 
		  int x;
		 
		 public:
		 
		  virtual void mfl() = 0;
		 
		  virtual void mfl(int);
		 
		  virtual void mf2();
		 
		  void mf3 ();
		 
		}
		 
		void mf3(double); class Derived: public Base {
		 
		 public:
		 
		  using Base::mfl; //使用using 声明式
		 
		  using Base: :mf3; //使用using 声明式
		 
		  virtual void mfl();
		 
		  void mf3 ();
		 
		  void mf4();
		 
		}
		 
		Derived d;
		 
		int x;
		 
		d.mf1 () ; //仍调用Derived: :mfl
		 
		d.mf1 (x); //调用Base: :mfl
		 
		d.mf2 () ; //调用Base: :mf2
		 
		d.mf3 ();//调用Derived: :mf3
		 
		d.mf3 (x); //调用Base: :mf3
	[2] 使用转变函数
	
		class Derived: private Base (
		 
		 public:
		 
		  virtual void mfl () //转变函数(forwading  function) ,
		 
		  { Base:: mfl ( );} //暗自成为inline
		 
		}
	
条款34: 区分接口继承和实现继承

	本条款告诉程序员：
	(1)	接口继承和实现继承不同。在public 继承之下， derived classes 总是继承base class
	的接口。
	(2) pure virtual 函数只具体指定接口继承。(要求继承者必须重新实现该接口)
	(3) 简朴的(非纯) impure virtual 函数具体指定接口继承及缺省实现继承（继承者可自己实现该接口也可使用缺省实现）。
	(4) non-virtual 函数具体指定接口继承以及强制性实现继承。（继承者必须使用该接口的实现）
	举例：
	
	class Shape {
	 
	 public:
	 
	  virtual void draw( ) const = 0; //pure virtual 函数
	 
	  virtual void error(const std::string& msg); //简朴的(非纯) impure virtual 函数
	 
	  int objectID ( ) const;// non-virtual 函数
	 
	};
	 
	class Rectangle: public Shape { };
	 
	class Ellipse: public Shape { };
	
条款35: 考虑virtual函数以外的其他选择

	本条款告诉程序员，当需要使用virtual 函数时，可以考虑其他选择。
	Virtual函数的替代方案是：
	(1)	使用non-virtual interface(NVI)手法。思想是：将virutal函数放在private中，而在public中使用一个non-virtual函数调用该virtual函数。优点是：用一个不能被子类重定义的函数，做一些预处理、后处理等，子类只需要在private中重新实现virtual函数即可。即：基类给出virtual函数的使用方法，而派生类给出virtual函数的使用方法。
	举例：
		class GameCharacter {
		 
		 public:
		 
		  int healthValue() const{                // 1. 子类不能重定义
		 
		    ...                               // 2. preprocess
		 
		    int retVal = doHealthValue();     // 2. 真正的工作放到虚函数中
		 
		    ...                               // 2. postprocess
		 
		   return retVal;
		 
		  }
		 
		  ...
		 
		 private:
		 
		  virtual int doHealthValue() const {   // 3. 子类可重定义
		 
		    ...
		 
		   }
		 
		};
	(2)	将virtual函数替换为“函数指针成员变量”（这是Strategy设计模式中的一种表现形式）。优点是对象实例和派生类对象，可使用各种实现，也可在运行时随意改；缺点是：该函数不能访问类中的私有成员
	举例：
	
		class GameCharacter;
		 
		int defaultHealthCalc(const GameCharacter& gc); // default algorithm
		 
		class GameCharacter {
		 
		 public:
		 
		  typedef int (*HealthCalcFunc)(const GameCharacter&);
		 
		  explicit GameCharacter(HealthCalcFunc hcf = defaultHealthCalc)
		 
		  : healthFunc(hcf)
		 
		  {}
		 
		  int healthValue() const {
		 
		   return healthFunc(*this);
		 
		  }
		 
		  ...
		 
		 private:
		 
		  HealthCalcFunc healthFunc;
		 
		};
	(3)	以tr1::function成员变量替换virtual函数，这允许使用任何可调用物搭配一个兼容于需求的签名式。这也是Strategy设计模式的某种形式。这种方式比上面的函数指针更灵活、限制更少：[1]返回值不一定是int，与其兼容即可; [2]可以是function对象; [3]可以是类的成员函数。
	(4)	继承体系内的virtual函数替换为另一个继承体系内的virtual函数。这是Strategy设计模式的传统实现手法。这种方式最大的优点是：可以随时添加新的算法。举例：
	
		class GameCharacter;
		 
		class HealthCalcFunc {
		 
		 public:
		 
		  ...
		 
		  virtual int calc(const GameCharacter& gc) const
		 
		   { ... }
		 
		  ...
		 
		};
		 
		HealthCalcFunc defaultHealthCalc;
		 
		class GameCharacter {
		 
		 public:
		 
		  explicit GameCharacter(HealthCalcFunc *phcf = &defaultHealthCalc)
		 
		   : pHealthCalc(phcf)
		 
		    {}
		 
		  int healthValue() const {
		 
		   return pHealthCalc->calc(*this);
		 
		 }
		 
		  ...
		 
		private:
		 
		HealthCalcFunc *pHealthCalc;
		 
		};
		
条款36: 绝对不要重新定义继承而来的non-virtual函数

	本条款告诫程序员：绝不要重新定义继承而来的non-virtual函数，因为这不仅容易造成错误，而且是一种自相矛盾的设计。 举例：
	
		class B{
		 
		 public:
		 
		  void func(){ cout<<“B”;}
		 
		};
		 
		class D:public B{
		 
		 public：
		 
		  void func() { cout<<“D”;}
		 
		};
	下面是对B和D的使用：
	
		D dObject;
		 
		B* basePtr = &dObject;
		 
		D* dOjbectPtr = &dObject;
	看下面这两种调用方式:
	
		basePtr->func();
		 
		dOjbectPtr->func();
	你会发现打印结果为：
	B
	D
	解释：在C++继承中，virtual函数是动态绑定的，调用的函数跟指针或者引用实际绑定的那个对象有关，而non-virtual函数是静态绑定的，调用的函数只跟声明的指针或者引用的类型相关。
	此外，继承者自己重新实现了non-virtual函数的行为是自相矛盾的。Non-virtual函数是用于同时指定函数接口和函数实现的，既然你想只继承函数接口，就应该定义为non-virtual的。

条款37: 绝对不要重新定义继承而来的缺省参数值

	该条款告诫程序员：绝对不要重新定义一个继承而来的缺省参数值,因为缺省参数值都是静态绑定,而virtual函数－你唯一应该覆写的东西－却是动态绑定。
	举例：
	
		class Shape{
		 
		 public:
		 
		  enum Color{RED,GREEN,BLUE};
		 
		  virtual void draw(Color color = RED)const = 0;
		 
		  ...
		 
		};
		 
		class Circle:public Shape{
		 
		 public:
		 
		  //竟然改变缺省参数值
		 
		  virtual void draw(Color color = GREEN)const{ ... }
		 
		};
		 
		Shape* pc = new Circle;
		 
		pc->draw(); //注意调用的是: Circle::draw(RED)，也就是说，此处的draw函数是基类和派生类的“混合物”。
	为什么缺省参数是静态绑定而不是动态绑定呢？主要原因是运行效率。如果是动态绑定，程序员使用起来很方便，但会降低运行效率，C++做了取舍，结果就是现在这样。
	
条款38:XXXXXXXXXXXXXXXXXXX

条款39:明智而审慎地使用private继承

	（1）如果class之间的继承关系是private。编译器不会自动将一个derived class对象转化为一个base class对象。由private base class继承而来的所有成员，在derived class中都会变成private属性，纵使它们在base class中原来是protected或public属性。
	（2）private继承意味is-implemented-in-terms-of，它的级别比组合低，当derived class需要protected base class或者需重新定义继承而来的virtual class时，设计才是合理的。
	（3）与复合不同 ，private继承可以使empty base空间最优化。举例：
	
		class Empty{}; //empy class
		 
		clsss HoldsAnyInt{
		 
		 private:
		 
		  int x;
		 
		  Empty e;
		 
		};//这个的大小为>sizeof(int),Empty空对象需要安插一个char到空对象，并且有齐位需求。
		 
		class HoldsAnyInt::private Empty{
		 
		 private:
		 
		  int x;
		 
		}; //这个sizeof大小为sizeof（int）
		 
		补充：
		 
		class HoldsAnyInt::private Empty{
		 
		 private:
		 
		  int cal() = 0;
		 
		  int x;
		 
		}; //这个sizeof大小为8, 实际上为size(int) + sizeof(vptr)
		
条款40:明智而审慎地使用多重继承

	(1)	多重继承比单一继承复杂。他可能导致新的歧义性，以及virtual继承的需要
	(2)	Virtual继承会增加大小、速度、初始化复杂度等等成本。如果virtual base classed不带任何数据，将是最具使用价值的情况。
	(3)	多重继承最正当用途是：其中一个设计“public 继承某个interface class”和“priavte继承某个协助实现的class”的两相结合。
###7. 模板与泛型编程（Templates and Generic Programming）
条款41:了解隐式接口和编译期多态

	(1)	class和templates都支持接口(interfaces)和多态(polymorphism)。
	(2)	对classes而言接口是显式的(explicit),以函数签名为中心。多态则是通过virtual函数发生于运行期。
	(3)	对template参数而言，接口是隐式的(implicit)，奠基于有效表达式；多态则是通过template具现化和函数重载解析(function overloading resolution)发生于编译期。
	条款42:了解typename的双重定义
	(1)	声明template参数时,前缀关键字class与typename可互换。
	例如：
	
		template <class T> //or template <typename T>
		 
		void swap(T& obj1, T& obj2) {
		 
		  T temp(obj1);
		 
		  obj1 = obj2;
		 
		  obj2 = temp;
		 
		}
	(2)	请使用关键字typename标识嵌套从属类型名称；但不得在base class lists(基类列)或member initailization list(成员初值表列)内以作为base class修饰符。
	例如，你必须：
	
		tempalte <typename C>
		 
		void print2nd(const C& container)//打印容器内的第二元素
		 
		{
		 
		  if( containter.size() >= 2 ){
		 
		   typename C::const_iterator iter( containter.begin() );
		 
		   ++iter;
		 
		   int value = *iter;
		 
		   std::cout << value;
		 
		  }
		 
		}
	解释：template内出现的名称如果相依于某个template参数，称之为从属名称；如果从属名称在class内呈嵌套状，称为嵌套从属名称。在上面的例子中，C::const_iterator就是嵌套从属名称。编译器并不知道 const_iterator是个类型，除非你告诉编译器，不然它以为这是C中的static成员变量或者是global变量。
	但需要注意一下情况：
	
		template <typename T>
		 
		class Derived:public Base<T>::Nested{ //base class list中不允许出现"typename"
		 
		 public:
		 
		  explicit Dervied(int x) : Base<T>::Nested(x){ //成员初始化列表中不允许"typename"
		 
		  typename Base<T>::Nested temp; //既不在base class list也不在初始化列表中,作为一个base class修饰符需加上typename.
		 
		  ...
		 
		  }
		 
		  ...
		 
		};
		
条款43:学习处理模板化基类内的名称

	本条款给出了以下问题的解决方案：当基类是模板化的类时，派生类应该怎样调用基类中的函数。
	
	
	template<typename Company>
	 
	class MsgSender{
	 
	 public:
	 
	  ...
	 
	  void sendClear(const MsgInfo& info){
	 
	   std::string msg;
	 
	   ...//根据info产生信息
	 
	   Company c;
	 
	   c.sendClearText(msg);
	 
	  }
	 
	  void sendSecret(const MsgInfo& info){...} //这里调用的是c.sendEncrypted.
	 
	};
	 
	template <typename Company>
	 
	class LoggingMsgSender:public MsgSender<Comany>{
	 
	 public:
	 
	  ...
	 
	  void sendClearMsg(const MsgInfo& info){ //为避免"名称遮掩"现象的发生,采用了一个不同的名称
	 
	   ...// record status information before sending message
	 
	   sendClear(info);
	 
	   ...//record status information after sending message.
	 
	  }
	 
	  ...
	 
	};
	以上代码直接编译会报错：抛出了”sendClear不存在”的抱怨。解决方法有以下三个：
	(1)	在base class函数调用动作之前加上”this->”：
	
	template <typename Company>
	 
	void LoggingMsgSender<Company>::sendClearMsg(const MsgInfo& info){
	 
	  ...
	 
	  this->sendClear(info); //ok
	 
	  ...
	 
	}
	(2)	使用using声明式：
	
	template <typename Company>
	 
	class LoggingMsgSender:public MsgSender<Company>{
	 
	 public:
	 
	  //这里的情况不是base class名称被derived class名称遮掩,而是编译器不进入base base
	 
	  //作用域查找,于是我们通过using声明式告诉它,请它这么做
	 
	  using MsgSender<Company>::sendClear;//告诉编译器,请它假设sendClear位于base class内
	 
	  ...
	 
	  void sendClearMsg(const MsgInfo& info){
	 
	   ...
	 
	   sendClear(info);//ok
	 
	   ...
	 
	  }
	 
	};
	(3)	明明白白指出被调用函数位于base class内:
	
	template <typename Company>
	 
	class LoggingMsgSender:public MsgSender<Company>{
	 
	 public:
	 
	  ...
	 
	  void sendClearMsg(const MsgInfo& info){
	 
	  ...
	 
	  MsgSender<Company>::sendClear(info); //ok
	 
	  ...
	 
	 }
	 
	 ...
	 
	};

条款44:将与参数无关的代码抽离template

	(1)	Templates生成多个classes和多个函数，所以任何template代码都不该与某个造成膨胀的template参数产生相依关系。
	(2)	因非类型模板参数而造成的代码膨胀，往往可消除，做法是以函数参数或class成员变量替换template参数。
	举个例子，假设现在你要为固定尺寸的矩阵编写一个template类,该类声明要支持矩阵的逆运算，可以采用下面代码：
	
	template <typename T, std::size_t n> //矩阵元素类型T，尺寸大小为n
	 
	class SquareMatrix{
	 
	 public:
	 
	   ...
	 
	   void invert(); //逆运算
	 
	};
	这样定义，声明以下两个对象会产生不同的代码，造成代码膨胀：
	
	SquareMatrix<double,5> square1;
	 
	SquareMatrix<double,10> square2;
	减小代码膨胀的方法是采用以下定义：
	
	template <typename T > //矩阵元素类型T
	 
	class SquareMatrix{
	 
	 public:
	 
	  ...
	 
	  void invert(std::size_t n); //把尺寸大小n作为参数
	 
	};
	
条款45：运用成员函数模板接受所有兼容类型

	本条款告诉你，怎样编写成员函数模板。从下面例子说起：
	怎样支持以下操作：
	
		template <typename T>
		 
		class SmartPtr{
		 
		 public:
		 
		  explicit SmartPtr(T* realPtr);//智能指针通常以原始指针完成初始化
		 
		  ...
		 
		};
		 
		SmartPtr<Top> top1_smart_ptr = SmartPtr<Middle>(new Middle);
		 
		SmartPtr<Top> top2_smart_ptr = SmartPtr<Bottom>(new Bottom);
		 
		SmartPtr<const Top> const_top2_ptr = top1_smart_ptr;
	一个比较好的方案是：
	
		//根据SmartPtr<U>创建一个SmartPtr<T>，其中T是基类，U是T的派生类
		 
		template <typename T>
		 
		class SmartPtr{
		 
		 public:
		 
		  template <typename U>
		 
		  SmartPtr(const SmartPtr<U>& other) :held_ptr_( other.get() ){...} //这里就完成子类向父类的隐式转换过程.
		 
		  T* get()const{ return held_ptr_;}
		 
		  ...
		 
		 private:
		 
		  T* held_ptr_; //这是SmartPtr持有的内置指针.
		 
		};
	上述中的SmartPtr构造函数便是成员函数模板（member function template），得出的结论是：
	(1)	请使用member function template(成员函数模板)生成”可接受所有兼容类型”的函数。
	(2)	如果你声明member template用于”泛化copy构造”或”泛化assignment操作”,你还需要声明正常copy构造函数和copy assignment操作符。（不然编译器会为你生成默认的copy构造函数和copy assignment操作符）

条款46：需要类型转换时请为模板定义非成员函数

	本条款告诉程序员，当你需要进行类型转化时，为了避免麻烦，最好将模板定义为非成员函数（如friend函数）。

条款47: 请使用traits classes 表现类型信息

条款48: 认识template 元编程

###8. 定制new和delete（Customizing new and delete）
条款49: 了解new-handler 的行为

	(1)	set_new_handler 允许客户指定一个函数，在内存分配无法获得满足时被调用。
	(2)	No-throw new 是一个颇为局限的工具，因为它只适用于内存分配;后继的构造函数调用还是可能抛出bad_alloc异常。


条款50: 了解new 和delete 的合理替换时机

	有许多理由需要写个自定的口new 和delete ，包括改善效能、对heap 运用错误进
	行调试、收集heap 使用信息。

条款51: 编写new和delete时需固守常规

	operator new内应该有一个无穷循环，并在其中尝试分配内存，如果分配失败，就调用new handler。它也应该有能力处理0 bytes申请（对于标准库中的new操作符，当用户申请0bytes，会返回1bytes的空间）。class版本还需要处理“比正确大小更大的（错误）申请”。
	需要注意的是，operator new成员函数会被derived classes继承，也就是说， base class的operator new可能被调用以分配derived class对象。因此 derived class的 operator new的代码建议：
	
	struct Base{
	 
	 static void* operator new(std::size_t size) throw( std::bad_alloc );
	 
	 ...
	 
	};
	 
	struct Derived:public Base{...};
	 
	Derived* p = new Derived;//call Base::operator new.
	 
	void* Base::operator new(std::size_t size) throw(std::bad_alloc)
	 
	{
	 
	  if( size != sizeof(Base) ){
	 
	  return ::operator new( size ); //call standard operator new version.
	 
	}
	 
	...
	 
	}

条款52: 写了placement new 也要写placement delete

	1.	当你写一个placement new，请确定也写出对应的placement delete。如果没这样做，你的程序可能会出现微弱时断时续的内存泄漏；
	2.	当你写placement new和placement delete时，请确定不要无意识的（非故意的）遮掩了全局范围默认的new/delete版本。
###9. 杂项讨论（Miscellany）

条款53: 不要轻忽编译器的警告

条款54: 让自己熟悉包括TR1在内的标准程序库

本条款告诉程序员：
1.C++标准程序库的主要机能由STL、iostreams、locales组成，并包括C99标准程序库。
2.TR1添加了智能指针、一般化函数指针、hash-based容器、正则表达式，以及另外10个组件的支持。
3.TR1自身只是一个规范。为获得tr1提供的好处，你需要一份实物。一个好的实物来源是boost。

条款55: 让自己熟悉Boost