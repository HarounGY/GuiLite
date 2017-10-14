# GuiLite - 你的辅助：不抢人头，只助攻!(开发者qq群：527251257)
超轻量UI框架 - GuiLite是由4千行左右的C/C++代码编写的超轻量UI框架。它像MFC，QT一样为软件开发人员提供界面支持。

由于代码量极少，它便于开发人员扩展自己的UI风格；由于其C/C++语言编写，它能够很方便的嵌入到其他UI系统中，比如嵌入到Android系统中，与Android界面风格并存，并运行在手机上面；或嵌入到Windows UWP系统中，与UWP界面风格并存，并运行在Windows桌面及虚拟/混合现实设备上；或嵌入到MFC，QT系统中，与MFC/QT界面风格并存，并运行在相关系统中；具体实现方法请见GuiliteSample库。

GuiLite渴望与所有平台共同发展，希望开发者能将自己界面中的固定部分用GuiLite开发，同时也积极利用MFC，QT，Android，Linux，Windows的界面特性，让自己的界面集百家之长，又不失个性。

相比其他强大的UI框架，GuiLite只聚焦界面开发（特别是手机风格的滑动界面），只使用最基础的C++特性，直达绘制底层。希望通过减少代码，调用层次，让UI框架的体积最小，效率更高，成为应用程序开发的得力配角。

注意：GuiLite作为框架，本身并不产生界面，界面的集成效果，请参考[GuiLiteSamples代码库](https://github.com/idea4good/GuiLiteSamples)
## GuiLite的基本原理
GuiLite只作两个工作：界面元素管理和图形绘制。

## 界面元素管理
界面元素管理包括：对所有控件（button，spinbox, lable, keyboard），容器（dialog, view）管理；具体的管理方法是为所有的界面元素建造一棵树，利用这棵树实现对所有界面元素的遍历，查询，状态更改。

比如：当你按下一个dialog的button时，手指的位置信息（x，y）会被传入树的根部（root），然后从root开始寻找，哪个dialog被点中，dialog的哪个button被点中，并调用buton被点中的回调函数，用于作相应处理（一般会进行button的状态修改及重绘工作）

### 界面元素如何创建
所有界面元素都继承自c_wnd类的对象，对象被实例化时，也就完成了界面元素的创建；但此时的界面元素是孤独的，与其他界面元素没有形成联系（没有父母，没有兄弟姐妹）

### 界面元素如何被管理（纳入tree中）
新创建的界面元素纳入管理的过程，就是为其添加父母，兄弟姐妹的过程。使用的函数接口为connect();从此该界面元素会跟其他界面元素一样，纳入一棵树中，并随之响应用户可能的点击操作。

当需要删掉该界面元素时，使用disconnect()；从此该界面元素会断绝所有的父子关系，从树上摘下来，不再响应用户的触控操作；但对象本身不会被销毁。典型应用场景：软键盘的创建/退出。

## 图形绘制
图形绘制包括： 绘制方法和图层管理。其中点绘制是线/面/位图绘制的基础，若干个点的绘制，形成点面及位图；图层管理，则是管理多个界面元素的遮挡关系，系统默认支持3层遮挡关系，这3个层次可以为：视图背景层，对话框层，keyboard/spinbox控件层。

### 绘制方法
请参看文件bitmap.cpp和surface.cpp中的draw_xxx()函数。

### 图层管理
GuiLite的所有图层，如下图所示：
![Graphic layer](GraphicLayer.png)

display层：
该层对应了物理显存，display层决定了一个显示终端的最终显示效果；通常系统中至少有一个display层。

surface层：
该层属于display层的一个部分；它为左右滑动而存在，每一张滑动页面均对应了一个surface层；surface层决定了一个滑动页面的最终显示效果；通常1个display层会对应多个display层。

frame层:
该层属于surface层的一个部分；它现实叠加界面元素而存在，
