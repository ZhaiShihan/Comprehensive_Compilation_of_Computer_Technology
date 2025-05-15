### 壹  准备工作

#### 述：
#####
#####

###### · 一个最简单的 Qt 应用程序：

· <font color="#ffc000">程序 1 - 1</font>：一个最简单的 Qt 应用程序
```C++
#include <QApplication>
#include <QWidget>

int main(int argc, char *argv[])
{
	QApplication a(argc, argv);
	QWidget w;
	w.show();
	
	return a.exec();
}
```

· Qt 头文件没有“.h”后缀
· Qt 一个类对应一个头文件，类名就是头文件名
· QApplication 应用程序类：
1. 管理图形用户界面应用程序的控制流和主要设置
2. 它是 Qt 的整个后台管理的命脉它**包含主事件循环**，在其中来自窗口系统和其它资源的**所有事件处理和调度**。它也处理**应用程序的初始化和结束**，并且**提供对话管理**
3. 对于任何一个使用 Qt 的图形用户界面应用程序，都正好存在一个 QApplication 对象，而不论这个应用程序在同一时间内是不是有 0、1、2 或更多个窗口
· a.exec()：程序进入消息循环，等待对用户输入进行响应；这里 main() 把控制权转交给 Qt，Qt 完成事件处理工作，当应用程序退出的时候 exec() 的值就会返回；**在exec() 中，Qt 接受并处理用户和系统的事件并且把它们传递给适当的窗口部件**

###### · 信号和槽机制：
· 信号槽是 Qt 框架的机制之一，所谓信号槽，实际就是观察者模式
· **当某个事件发生之后**，比如，按钮检测到自己被点击了一下，**它就会发出一个信号（signal）**；这种发出是没有目的的，类似广播
· **如果有对象对这个信号感兴趣，它就会使用连接（connect）函数**，意思是，将想要处理的信号和自己的一个函数（称为槽（slot））绑定来处理这个信号
· 也就是说，**当信号发出时，被连接的槽函数会自动被回调**
· 这就类似观察者模式：当发生了感兴趣的事件，某一个操作就会被自动触发

· `connect()` 函数最常用的一般形式：
```C++
connect(sender, signal, receiver, slot);

解释：
connect(发出信号的对象，发出的信号，接收信号的对象，接收对象在接收信号后调用的函数)
```

· <font color="#ffc000">程序 1 - 2</font>：Qt5 中信号槽函数简单示例
```C++
#include "mainwindow.h"
#include <QPushButton>
#include <QApplication>

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    MainWindow w;
    w.show();

    QPushButton button_A("quit");
    QObject::connect(&button_A, &QPushButton::clicked,
                    &a, QApplication::quit);

    button_A.show();
    return a.exec();

// 程序运行的时候会看到一个窗口和窗口下面一个不会随着窗口动的“quit”按钮
}
```

###### · 自定义信号槽：
· Qt 的信号槽机制并不仅仅是使用系统提供的那部分，还会允许用户自己设计自己的信号和槽

· <font color="#ffc000">程序 1 - 3</font>：
```C++
#include <QObject>
////////// newspaper.h //////////
class Newspaper: public QObject
{
	Q_OBJECT
	public:
		Newspaper(const QString &name): m_name(name) {}
		void send() {emit newPaper(m_name);}
	signals:
		void newPaper(const QString &name);
	private:
		QString m_name;
};

////////// reader.h //////////
#include <QObject>
#include <QDebug>

class Reader: public QObject
{
	Q_OBJECT
	public:
		Reader() {}
		void receiveNewspaper(const QString &name)
		{
			qDebug()<<"Receives Newspaper: "<<name;
		}
};

////////// main.cpp //////////
#include <QApplication>

#include "newspaper.h"
#include "reader.h"

int main(int argc, char *argv[])
{
	QApplication app(argc, argv);

	Newspaper newspaper("Newspaper A");
	Reader reader;
	QObject::connect(&newspaper, &Newspaper::newPaper, &reader, &Reader::receiveNewspaper);
	newspaper.send();

	return app.exec();
}
```

· 注意：
· 只有继承了 QObject 类的类，才具有信号槽的能力，所以为了使用信号槽，必须继承 QObject
· 凡是 QObject 类（不管是直接子类还是间接子类），都应该在第一行代码写上 `Q_OBJECT`

· *Newspaper 类的分析*：
· Newspaper 类的 public 和 private 代码块都比较简单，只不过它新加了一个 signals：signals 块所列出的，就是该类的信号
· 信号就是一个个的函数名，返回值是 void（因为无法获得信号的返回值，所以也就无需返回任何值），参数是该类需要让外界知道的数据
· 信号作为函数名，不需要在 cpp 函数中添加任何实现

· `emit` 是 Qt 对 C++ 的扩展，是一个关键字（其实也是一个宏）
· `emit` 的含义是发出，也就是发出 `newPaper()` 信号

· Qt 5 中，任何成员函数、static 函数、全局函数和 Lambda 表达式都可以作为槽函数
· 与信号函数不同，槽函数必须自己完成实现代码；槽函数就是普通的成员函数，因此作为成员函数，也会受到 public、private 等访问控制符的影响

· 自定义信号槽需要注意的事项;
· 发送者和接收者都需要是 QObject 的子类（当然，槽函数是全局函数、Lambda 表达式等无需接收者的时候除外）
· 使用 signals 标记信号函数，信号是一个函数声明，返回 void，不需要实现函数代码
· 槽函数是普通的成员函数，作为成员函数，会受到 public、private、protected 的影响
· 使用 emit 在恰当的位置发送信号
· 使用 `QObject::connect()` 函数连接信号和槽
· 任何成员函数、static 函数、全局函数和 Lambda 表达式都可以作为槽函数

###### · 信号槽的更多用法：
1. 一个信号可以和多个槽相连：如果是这种情况，这些槽会一个接一个的被调用，但是它们的*调用顺序是不确定的*
2. 多个信号可以连接到一个槽：只要任意一个信号发出，这个槽就会被调用
3. 一个信号可以连接到另外的一个信号：当第一个信号发出时，第二个信号被发出，除此之外这种“信号-信号”的形式和“信号-槽”的形式没有什么区别
4. 槽可以被取消链接，这种情况并不经常出现，因为当一个对象 delete 之后，Qt 自动取消所有连接到这个对象上面的槽
5. 使用 Lambda 表达式：在使用 Qt5 的时候，能够支持 Qt5 的编译器都是支持 Lambda 表达式的

###### · 其他信号触发方式：
例 1：在文本框中输入进程，点击 sure 执行
```C++
////////// weiget.cpp 中 //////////
void Widget::on_Sure_button_clicked()
{
    QString program = ui->lineEdit->text();    //获取文本框中输入的数据
    QProcess *myprocess = new QProcess(this);    //创建进程 process 对象
    myprocess->start(program);    //执行 process 内容
}
void Widget::on_cancel_button_clicked()
{
    this->close();    //关闭
}
void Widget::on_search_button_clicked()
{
	QMessageBox::information(this, "信息", "点击浏览");
	// QMessageBox 需要头文件 <QMessageBox>
}


////////// 头文件（widget.h）中 //////////
private slots:
    void on_Sure_button_clicked();
    void on_cancel_button_clicked();
    void on_search_button_clicked();
```

```C++
////////// weiget.cpp 中 //////////
Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
{
    ui->setupUi(this);

    //连接信号与槽：谁发出信号，发出什么信号，谁处理信号，怎么处理
    connect(ui->lineEdit, SIGNAL(returnPressed()), this, SLOT(on_commitButton_clicked()));    //回车与“点击确定按钮”绑定

	connect(ui->cancel_button, SIGNAL(clicked), this, SLOT(on_cancel_button_clicked()));    //点击取消键的指令

    connect(ui->search_button, SIGNAL(clicked), this, 
SLOT(on_search_button_clicked()));    //点击“浏览”弹出对话框，标题为“信息”，内容为“点击浏览”
}
```

###### · Lambda 表达式：
· C++11 中的 Lambda 表达式**用于定义并创建匿名的函数对象**，以简化编程工作
· Lambda 表达式的基本构成：
```C++
[=] () mutable exception -> int
{
	int n=x+y;
	x=y;
	y=n;
	return n;
}

解释：
[函数对象参数] (操作符重载函数参数) mutable 或 exception -> 返回值 {函数体}
```

1. 函数对象参数：
· `[]` 标识一个 Lambda 的开始，这部分必须存在，不能省略
· 函数对象参数是传递给编译器自动生成的函数对象类的构造函数的
· 函数对象参数只能使用那些到定义 Lambda 为止时 Lambda 所在作用范围内可见的局部变量（包括 Lambda 所在类的 this）
· 函数对象参数有以下形式：
	1. 空：没有使用任何函数对象参数
	2. =：函数体内可以使用 Lambda 所在作用范围内所有可见的局部变量（包括 Lambda 所在类的 this），并且是*值传递方式*（相当于编译器自动为我们按值传递了所有局部变量）
	3. &：函数体内可以使用 Lambda 所在作用范围内所有可见的局部变量（包括 Lambda 所在类的 this），并且是*引用传递方式*（相当于编译器自动为我们按引用传递了所有局部变量）
	4. this：函数体内可以使用 Lambda 所在类中的成员变量
	5. a：将 a 按值进行传递，按值进行传递时，函数体内不能修改传递进来的 a 的拷贝，因为默认情况下函数是 const 的；要修改传递进来的 a 的拷贝，可以添加 mutable 修饰符
	6. a, &b：将 a 按值进行传递，b 按引用进行传递
	7. =, &a, &b：除 a 和 b 按引用进行传递外，其他参数都按值进行传递
	8. &, a, b：除 a 和 b 按值进行传递外，其他参数都按引用进行传递

· 示例：
```C++
int m = 0, n = 0;
[=] (int a) mutable {m=++n+a;}(4);
[&] (int a) {m=++n+a;}(4);
[=,&m] (int a) mutable {m=++n+a;}(4);
[&,m] (int a) mutable {m=++n+a;}(4);
[m,n] (int a) mutable {m=++n+a;}(4);
[&m,&n] (int a) {m=++n+a;}(4);
```

2. 操作符重载函数参数：
· 标识重载的 () 操作符的参数，没有参数时，这部分可以省略；参数可以通过按值（如：(a,b)）和按引用（如：(&a, &b)）两种方式进行传递

3. 可修改标示符：
· mutable 声明，这部分可以省略；按值传递函数对象参数时，加上 mutable 修饰符后，可以修改按值传递进来的拷贝（注意是能修改拷贝，而不是值本身）

4. 错误抛出标示符：
· exception 声明，这部分也可以省略；exception 声明用于指定函数抛出的异常，如抛出整数类型的异常，可以使用 throw(int)

5. 函数返回值：
· -> 返回值类型，标识函数返回值的类型，当返回值为 void，或者函数体中只有一处 return 的地方（此时编译器可以自动推断出返回值类型）时，这部分可以省略

6. 是函数体：
· { }，标识函数的实现，这部分不能省略，但函数体可以为空

### 贰  窗口系统

#### 述：
#####
#####

###### · Qt 窗口坐标体系：
· 以左上角为原点，$X$ 向右增加，$Y$ 向下增加
· 对于嵌套窗口，其坐标是**相对于父窗口**来说的
![|350](Qt%20开发基础图/Qt%20开发基础图1.png)
                               （图一：Qt 窗口坐标体系）

###### · QWidget：
· 所有窗口及窗口控件都是从 QWidget 直接或间接派生出来的
· 当创建一个 QObject 对象时，会看到 QObject 的构造函数接收一个 QObject 指针作为参数，这个参数就是 parent，也就是父对象指针；这相当于，在创建 QObject 对象时，可以提供一个其父对象，我们创建的这个 QObject 对象会自动添加到其父对象的 children() 列表
· 当父对象析构的时候，这个列表中的所有对象也会被析构
· 这种机制在 GUI 程序设计中相当有用：例如，一个按钮有一个 QShortcut（快捷键）对象作为其子对象，当删除按钮的时候，这个快捷键理应被删除

· QWidget 是能够在屏幕上显示的一切组件的父类，继承自 QObject，因此也继承了这种对象树关系：一个孩子自动地成为父组件的一个子组件；因此它会显示在父组件的坐标系统中，被父组件的边界剪裁；例如当用户关闭一个对话框的时候，应用程序将其删除，那么，我们希望属于这个对话框的按钮、图标等应该一起被删除。事实就是如此，因为这些都是对话框的子组件
· 也可以自己删除子对象，它们会自动从其父对象列表中删除：比如当删除了一个工具栏时，其所在的主窗口会自动将该工具栏从其子对象列表中删除，并且自动调整屏幕显示

· 当一个 QObject 对象在堆上创建的时候，Qt 会同时为其创建一个对象树；不过对象树中对象的顺序是没有定义的，这意味着销毁这些对象的顺序也是未定义的
· 任何对象树中的 QObject 对象 delete 的时候，如果这个对象有 parent，则自动将其从 parent 的 children() 列表中删除；如果有孩子，则自动 delete 每一个孩子；Qt 保证没有 QObject 会被 delete 两次，这是由析构顺序决定的

###### · QMainWindow：
· QMainWindow 是一个为用户提供主窗口程序的类，包含一个菜单栏（menu bar）、多个工具栏（tool bars）、多个锚接部件（dock widgets）、一个状态栏（status bar）及一个中心部件（central widget），是许多应用程序的基础，如文本编辑器，图片编辑器等

![|400](../../Pasted%20image%2020240821161154.png)
                           （图二：QMainWindow 的结构）
1. 菜单栏（Menu bar）：
· 一个主窗口最多只有一个菜单栏。位于主窗口顶部、主窗口标题栏下面
· 创建菜单栏，通过 QMainWindow 类的 menubar（）函数获取主窗口菜单栏指针：
```C++
QMenuBar *menuBar() const
```
· 创建菜单，调用 QMenu 的成员函数 addMenu 来添加菜单：
```C++
QAction *addMenu(QMenu *menu)
QMenu *addMenu(const QString &title)
QMenu *addMenu(const QIcon &icon, const QString &title)
```
· 创建菜单项，调用 QMenu 的成员函数 addAction 来添加菜单项
```C++
QAction *activeAction() const
QAction *addAction(const QString &text)

QAction *addAction(const QIcon &icon, const QString &text)

QAction *addAction(const QString &text, const QObject *receiver,
 const char *member, const QKeySequence &shortcut=0)

QAction *addAction(const QIcon &icon, const QString &text,
 const QObject *receiver, const char *member,
 const QKeySequence &shortcut=0)
```
· Qt 并没有专门的菜单项类，只是使用一个 QAction 类，抽象出公共的动作；当我们把 QAction 对象添加到菜单，就显示成一个菜单项，添加到工具栏，就显示成一个工具按钮；用户可以通过点击菜单项、点击工具栏按钮、点击快捷键来激活这个动作

2. 工具栏（Tool bar）：
· 主窗口的工具栏上可以有多个工具条，通常采用一个菜单对应一个工具条的的方式，也可根据需要进行工具条的划分
· 直接调用 QMainWindow 类的 addToolBar() 函数获取主窗口的工具条对象，每增加一个工具条都需要调用一次该函数
· 插入属于工具条的动作，即在工具条上添加操作：通过 QToolBar 类的 addAction 函数添加
· 工具条是一个可移动的窗口，它的停靠区域由 QToolBar 的 allowAreas 决定，包括：
```C++
Qt::LeftToolBarArea    停靠在左侧
Qt::RightToolBarArea    停靠在右侧
Qt::TopToolBarArea    停靠在顶部
Qt::BottomToolBarArea    停靠在底部
Qt::AllToolBarAreas    以上四个位置都可停靠

# 使用 setAllowedAreas() 函数指定停靠区域：
setAllowedAreas(Qt::LeftToolBarArea | Qt::RightToolBarArea)

# 使用 setMoveable() 函数设定工具栏的可移动性：
setMoveable(false)    //工具条不可移动, 只能停靠在初始化的位置上
```

3. 状态栏（Status bar）：
· 派生自 QWidget 类，使用方法与 QWidget 类似，QStatusBar 类常用成员函数：
```C++
//添加小部件
void addWidget(QWidget *widget, int stretch=0)

//插入小部件
int insertWidget(int index, QWidget *widget, int stretch=0)

//删除小部件
void removeWidget(QWidget *widget)
```

###### · 资源文件：


```
User            UI            Reg           DB
 |              |              |             |
 |--注册请求-->  |              |             |
 |              |--请求验证---> |             |
 |              |              |--写入数据--->|
 |              |              |<--返回结果---|
 |              |<--显示结果--- |             |
 |<--显示反馈--- |              |             |


Doc-UI         doc           DBUtils         DB
 |              |              |             |
 |--进入操作-->  |              |             |
 |              |--功能请求---> |             |
 |              |              |--写入数据--->|
 |              |              |<--返回结果---|
 |              |<--请求反馈--- |             |
 |<--显示------- |              |             |

Pat-UI         pat           DBUtils         DB
 |              |              |             |
 |--进入操作-->  |              |             |
 |              |--功能请求---> |             |
 |              |              |--写入数据--->|
 |              |              |<--返回结果---|
 |              |<--请求反馈--- |             |
 |<--显示------- |              |             |

```