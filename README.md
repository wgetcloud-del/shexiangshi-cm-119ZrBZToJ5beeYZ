
我曾在随笔《[基于Python后端构建多种不同的系统终端界面研究](https://github.com/wuhuacong/p/18455704 "发布于 2024-10-10 11:16")》介绍了多种系统终端界面开发的处理，其中涉及到的wxpython，是一个非常不错的原生界面效果组件，我们可以通过利用其各种界面控件，结合Python跨平台运行的特性，为Windows、MacOS、Ubuntu等Linux系统，开发一套界面效果一致的应用系统。


我们可以基于VSCode\+wxpython\+wxFormBuilder组合实现桌面端的开发，可以利用wxFormBuilder来快速生成一些界面效果进行重用，wxFormBuilder类似WinForms里面的窗体设计器，完成设计后生成Python的类代码即可在项目中直接使用。


下面是相关的资源地址：


wxpython：[https://www.wxpython.org/](https://github.com)


wxFormBuilder： [https://github.com/wxFormBuilder/wxFormBuilder](https://github.com)


以及一些Github上的案例项目：


[https://github.com/janbodnar/wxPython\-examples](https://github.com):[豆荚加速器](https://baitenghuo.com) 


 wxPython 帮助文档和案例：[https://extras.wxpython.org/wxPython4/extras](https://github.com)


wxPython基础教程：[https://www.w3ccoo.com/wxpython/index.html](https://github.com)


在开始介绍WxPython界面组件之前，我们先来了解一下案例的界面效果，然后在逐步深入了解。由于它可以再不同的系统上运行，因此我们分别截图MacOS、Windows系统的界面效果来介绍。


下面是在不同的系统中创建一些主体界面元素作为参考。


**MacOS中的界面效果：**


![](https://img2024.cnblogs.com/blog/8867/202410/8867-20241029103658364-432406647.png)


**Windows系统的界面效果：**


![](https://img2024.cnblogs.com/blog/8867/202410/8867-20241029110816892-1001308159.png)


 两者界面很类似，这里包含了常用的菜单、工具栏、状态栏、多面板和多文档界面等界面元素，作为主体框架界面，剩下的根据不同的业务，构建列表展示界面、编辑/查看记录界面等常规处理即可。


### 1、wxPython 中主要类的继承关系


为了研究wxpython的各个界面元素，我们需要了解一下它们之间的关系。


以下是 wxPython 中主要类的继承关系和一些重要类的详细列表：


#### 1\. **wx.Object**


* 基类，所有 wxPython 类的基础。


#### 2\. **wx.EvtHandler**


* 处理事件的基类。
* 主要派生类：
	+ **wx.Window** (也作为 wx.Control 的基类)


#### 3\. **wx.Window**


* 所有窗口类的基类。
* 主要派生类：
	+ **wx.Frame**: 主框架窗口。
	+ **wx.Dialog**: 对话框窗口。
	+ **wx.Panel**: 面板，容纳其他控件。
	+ **wx.MDIParentFrame**: MDI（多文档界面）父窗口。
	+ **wx.MDIChildFrame**: MDI 子窗口。
	+ **wx.ScrolledWindow**: 支持滚动的窗口。


![](https://img2024.cnblogs.com/blog/8867/202410/8867-20241029111407734-944388793.png)


#### 4\. **wx.Control**


* 控件的基类。
* 主要派生类：
	+ **wx.Button**: 按钮控件。
	+ **wx.TextCtrl**: 文本输入框。
	+ **wx.CheckBox**: 复选框。
	+ **wx.RadioButton**: 单选框。
	+ **wx.ListBox**: 列表框。
	+ **wx.ComboBox**: 下拉框。
	+ **wx.Slider**: 滑块控件。
	+ **wx.StaticText**: 静态文本显示控件。
	+ **wx.StaticBitmap**: 静态位图显示控件。
	+ **wx.Choice**: 选择框控件。
	+ **wx.TreeCtrl**: 树形控件。
	+ **wx.ListCtrl**: 列表控件


![](https://img2024.cnblogs.com/blog/8867/202410/8867-20241029111516866-304746241.png)


#### 5\. **wx.Sizer**


* 布局管理类的基类。
* 主要派生类：
	+ **wx.BoxSizer**: 水平或垂直排列控件。
	+ **wx.GridSizer**: 网格布局。
	+ **wx.FlexGridSizer**: 灵活网格布局。
	+ **wx.StaticBoxSizer**: 包含静态框的布局。
	+ **wx.WrapSizer**: 自动换行的布局管理器。


![](https://img2024.cnblogs.com/blog/8867/202410/8867-20241029113432583-1324045833.png)


#### 6\. **wx.Menu**


* 菜单类。
* 主要派生类：
	+ **wx.MenuBar**: 菜单栏类。
	+ **wx.PopupMenu**: 弹出菜单类。


#### 7\. **wx.ToolBar**


* 工具栏类。


#### 8\. **wx.StatusBar**


* 状态栏类。


#### 9\. **wx.Notebook**


* 选项卡控件，允许用户在多个页面之间切换。


#### 10\. **wx.AuToolBar**


* 自定义工具栏类，支持多种风格和特性。


![](https://img2024.cnblogs.com/blog/8867/202410/8867-20241029111741828-766377656.png)


另外还提供了RibbonBar的工具栏效果，如下所示


![](https://img2024.cnblogs.com/blog/8867/202410/8867-20241029111903893-508817719.png)


如果我们使用SplitterWindow,那么可以把界面分拆为几个部分。


![](https://img2024.cnblogs.com/blog/8867/202410/8867-20241029113758982-427475216.png)


 这些类提供了 wxPython 的基础功能，使得开发者能够构建复杂的 GUI 应用程序。每个类都有自己的一组方法和属性，可以参考 wxPython 的官方文档来了解具体的使用方式和示例。


![](https://img2024.cnblogs.com/blog/8867/202410/8867-20241029133749929-1876495278.png)


 


### 2、案例的界面代码分析


了解了案例界面后，我们可以看看他们的界面代码是如何处理的。




```
class AUIDemoApp(wx.Frame):
    """带AUI管理的多文档主窗口"""

    id_open = wx.NewIdRef()
    id_save = wx.NewIdRef()
    id_quit = wx.NewIdRef()

    id_help = wx.NewIdRef()
    id_about = wx.NewIdRef()

    def __init__(self, parent):
        # super(AUIDemoApp, self).__init__(None, title="AUI 多文档界面", size=(1000, 700))

        wx.Frame.__init__(self, parent, style=wx.DEFAULT_FRAME_STYLE)
        self.SetTitle("AUI 多文档界面，包含菜单、工具栏、状态栏")
        self.SetIcon(appIcon.Icon)
        self.SetBackgroundColour((224, 224, 224))  # 设置窗口背景色
        self.SetSize((1000, 700))
        wx.Image.SetDefaultLoadFlags(0)  # 解决图片加载问题

        self._init_ui()
        self.Center()
```


这个部分是主界面窗口的初始化函数，其中设置标题、图标、背景色、以及尺寸等，并通过 \_init\_ui()函数来实现所有界面元素的初始化，如工具栏、菜单栏、状态栏，以及多文档界面的布局。


初始化界面的代码主要内容如下所示。


![](https://img2024.cnblogs.com/blog/8867/202410/8867-20241029114234160-147490679.png)


 这里使用 aui.AuiManager 来控制多个可以停靠面板的显示处理。菜单栏的初始化的代码，如下所示。




```
    def _create_menubar(self):
        """创建菜单栏"""

        self.mb = wx.MenuBar()

        # 文件菜单
        m = wx.Menu()
        m.Append(self.id_open, "打开文件")
        m.Append(self.id_save, "保存文件")
        m.AppendSeparator()
        m.Append(self.id_quit, "退出系统")
        self.mb.Append(m, "文件")

        # self.Bind(wx.EVT_MENU, self.on_open, id=self.id_open)
        # self.Bind(wx.EVT_MENU, self.on_save, id=self.id_save)
        # self.Bind(wx.EVT_MENU, self.on_quit, id=self.id_quit)

        # 帮助菜单
        m = wx.Menu()
        m.Append(self.id_help, "帮助主题")
        m.Append(self.id_about, "关于...")
        self.mb.Append(m, "帮助")

        # self.Bind(wx.EVT_MENU, self.on_help, id=self.id_help)
        # self.Bind(wx.EVT_MENU, self.on_about, id=self.id_about)

        self.SetMenuBar(self.mb)
```


状态栏的代码处理如下所示。




```
    def createStatusBar(self):
        # 创建自定义状态栏
        self.status_bar = wx.StatusBar(self)
        self.SetStatusBar(self.status_bar)
        self.status_bar.SetFieldsCount(3)  # 设置字段数量
        self.status_bar.SetStatusText("Ready", 0)  # 第一个字段
        self.status_bar.SetStatusText("No document opened", 1)  # 第二个字段

        # 获取今天的日历
        calendar_today = getCalendar_today()
        self.status_bar.SetStatusText(calendar_today, 2)  # 第三个字段
```


工具栏界面代码初始化如下所示。




```
    def _create_toolbar(self, d="H"):
        """创建工具栏"""
        # 工具栏图标,使用嵌入的图片
        # bmp_open = wx.Bitmap("wxpython/images/32/calc.png", wx.BITMAP_TYPE_PNG)
        bmp_open = book.Bitmap
        bmp_save = clock.Bitmap
        bmp_help = email.Bitmap
        bmp_about = home.Bitmap

        if d.upper() in ["V", "VERTICAL"]:
            tb = aui.AuiToolBar(
                self,
                -1,
                wx.DefaultPosition,
                wx.DefaultSize,
                agwStyle=aui.AUI_TB_TEXT | aui.AUI_TB_VERTICAL,
            )
        else:
            tb = aui.AuiToolBar(
                self, -1, wx.DefaultPosition, wx.DefaultSize, agwStyle=aui.AUI_TB_TEXT
            )
        tb.SetToolBitmapSize(wx.Size(16, 16))

        tb.AddSimpleTool(self.id_open, "打开", bmp_open, "打开文件")
        tb.AddSimpleTool(self.id_save, "保存", bmp_save, "保存文件")
        tb.AddSeparator()
        tb.AddSimpleTool(self.id_help, "帮助", bmp_help, "帮助")
        tb.AddSimpleTool(self.id_about, "关于", bmp_about, "关于")

        tb.Realize()
        return tb
```


其中注释的代码： bmp\_open \= wx.Bitmap("wxpython/images/32/calc.png", wx.BITMAP\_TYPE\_PNG)


是用来加载图片路径的资源，有时候我们为了减少一些常见图片文件的依赖，我们可以把他们放在统一的地方，使用代码的方式处理


如下代码所示，使用PyEmbeddedImage来直接嵌入图片的Base64代码。




```
book = PyEmbeddedImage(
    b"""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"""
)
```


主界面中放置了一个多文档的文档对象，使用aui.NoteBook界面对象即可，如下代码所示。




```
        # 创建AuiNotebook用于多文档管理
        self.notebook = aui.AuiNotebook(
            self, style=aui.AUI_NB_CLOSE_ON_ALL_TABS | aui.AUI_NB_TAB_SPLIT
        )
        self.mgr.AddPane(self.notebook, aui.AuiPaneInfo().CenterPane())
```


如果我们想初始化一个窗体界面在AuiNoteBook组件中，那么如下所示。




```
        # 创建初始文档面板
        self.document_panel = DocumentPanel(self)
        icon = wx.ArtProvider.GetBitmap(wx.ART_FOLDER, wx.ART_OTHER, (16, 16))
        self.notebook.AddPage(self.document_panel, "Document", select=True, bitmap=icon)
```


当然，我们可以进一步优化上面的实例代码，使得工具栏、菜单栏的内容动态根据配置生成界面，这样就可以避免手工添加的麻烦。


后续会继续根据我Winform开发框架的一些界面设计效果，进一步完善基于Wxpython组件界面的处理，构建一个基于Python后端Web API或者本地多种数据库操作的通用应用系统的管理，包括用户、角色、机构、权限、日志、菜单、字典、附件等基础框架内容的处理效果。


