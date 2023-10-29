内部类
=======

这一部分可以参考 ``include\Game\GameSystem.h`` 和 ``src\GameSystem.cpp`` 文件内容。

SceneBase
~~~~~~~~~

场景类型的基类，所有的场景类都要是这个的子类。

GameImage
~~~~~~~~~

Image类型，可以类比为RMXP里的 ``Sprite`` 类（虽然这个就是SFML的 ``Sprite`` 的封装）。

原理是在执行带参数构造函数或者 ``setSprite`` 后，将 ``this`` 添加到 ``motaGraphics`` 的队列当中，每次更新画面将会逐一显示，只需要调整这个对象的各参数即可。

成员
-----

.. csv-table:: 
    :widths: 30, 100

    "x,y,z", "Image的xy坐标，z是优先级，z越高在屏幕上的优先级越大，显示的就越靠前"
    "origin_x,origin_y", "设置图像的原点，默认都为0"
    "scale_x,scale_y", "设置图像在横纵方向上的放缩率"
    "width,height,sx,sy", "用于切割图像的参数，默认都为0，width和height为0时不进行切割"
    "opacity", "图像的不透明度"
    "angle", "图像的旋转角度"
    "visible", "决定一个图像是否可见"

函数
-----

构造函数
^^^^^^^^
GameImage(file,ix,iy,iwidth=0,iheight=0,isx=0,isy=0)，对相应的成员进行初始化。

除了 ``file`` 以外，其余参数缺省值为0。

setSprite(file,ix,iy,iwidth,iheight,isx,isy)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

对 ``Image`` 的 ``Sprite`` 进行设置（更新），除了 ``file`` 以外，其余参数缺省值为0。

dispose()
^^^^^^^^^

将该对象从 ``motaGraphics`` 的队列中删除，更新画面时将不再显示。

System -> motaSystem
~~~~~~~~~~~~~~~~~~~~~

本框架的系统类，用于储存各种和系统相关的数据，对应跨文件全局变量为 ``motaSystem`` 。

成员
----

.. csv-table:: 
    :widths: 50, 200

    "window", "为这个游戏的窗口，一切和窗口相关的都在这里调整"
    "font", "为游戏字体，字体的设置在这里调整"
    "bgm", "为游戏BGM，Music类型变量"
    "scene", "为设置场景的变量，motaSystem.scene可以类比为RMXP里的$scene"
    "gameTime", "用以计算游戏时间，每刷新一次将会+1，一般是用来设置行走动画相关的内容"
    "frameRate", "是刷新频率，每隔若干时间会刷新一次"
    "windowOpacity", "是默认的窗口不透明度"
    "resolutionRatio", "是当前的分辨率（放大率），最低是1（即默认的640x480），最大是2（变成1280x960）"
    "BGMVolume,SEVolume", 为BGM和SE的音量。
    "xxSE", "为各种SE的文件名，具体参考main.ini"
    "textureCache", "为纹理的缓存，这些都会在开始游戏时缓存完成。"

函数
----

init()
^^^^^^^

对各种成员初始化的函数，读取系统数据也是在这里进行。

bgmSwitch(file)
^^^^^^^^^^^^^^^

切换BGM使用的函数。

GameKeyBoard -> motaKeyBoard
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

关于全键盘的类，和魔塔样板的全键盘脚本相似，对应跨文件全局变量为 ``motaKeyBoard`` 。

press(key)
----------

原理是 ``GetAsyncKeyState(key)&0x8000`` ，返回值看是否非零，这样可以无缓存的判定键盘的按下，只要你的手指还按在键盘上，就会不断地响应。

repeat(key)
------------

原理是用计数器统计 ``GetAsyncKeyState(key) & 0x8000`` 相应的次数，若干次内将不再返回 ``true`` 。

trigger(key)
------------

原理是在按下时记录一次，松开时取消记录，而有记录时将不再相应，按一次键盘无论按多久都只响应一次。

pressConfirm(),repeatConfirm(),triggerConfirm()
-----------------------------------------------

使用上述三种按法按下确认键，将会返回 ``true`` 。

确认键指的是 ``Enter`` 和 ``Space`` 。

pressCancel(),repeatCancel(),triggerCancel()
---------------------------------------------

使用上述三种按法按下取消键，将会返回 ``true`` 。

取消键指的是 ``X`` 和 ``Esc`` 。

doubleClick(key)
----------------

判定是否双击的函数，原理是判定两次连续的敲击键盘，如果时间间隔不超过若干毫秒，则判定为双击。

dir4()
------

判定四方向的函数，下左右上分别返回0,1,2,3，否则返回-1。

GameWindows
~~~~~~~~~~~

游戏窗口的类，同理，也是在构造函数之后将 ``this`` 添加到 ``motaGraphics`` 当中。

成员
----

x,y,z,width,height,opacity,visible
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

窗口的x,y,z坐标、宽高和不透明度以及是否可见。

hasFunction
^^^^^^^^^^^^

判断是否有执行函数的变量，如果为 ``true`` ，则在显示的时候会执行里面的 ``refresh()`` 函数。

函数
-----

构造函数
^^^^^^^^^

GameWindow(rect,wopacity)， ``rect`` 为信息矩形，分别为xy坐标和宽高， ``opacity`` 为不透明度，缺省值窗口默认不透明度。

drawRect(rect)
^^^^^^^^^^^^^^

在窗口绘制选择矩形框的函数。

drawWText
^^^^^^^^^^

在窗口里面显示文字的函数，默认原点为窗口左上角坐标。

其中 ``size`` 是字号， ``bond`` 为是否加粗， ``colour`` 是字体颜色。

drawWText(dx,dy,content,size=20L,bond=false,color=Color::White)
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

这是最简单的类型，只有 ``dx`` 和 ``dy`` 的设置，直接显示在屏幕上。

drawWText(rect,content,pos=0,size=20L,bond=false,color=Color::White)
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

为描绘文字添加了矩形限制 ``rect`` ，按照 ``pos`` 给定的位置来描绘， ``pos=0`` 时在矩形的左上角， ``pos=1`` 时在矩形的 **正中央** ， ``pos=2`` 时在矩形的右上角。

drawWTextRotate(rect,content,pos=0,size=20L,bond=false,colour=Color::White,angle=0)
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

为文本描绘提供旋转角度的函数， ``angle`` 是文字旋转角度。

drawWTextSelect(rect,content,pos=0,size=20L,bond=false,color=Color::White)
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

为文本描绘添加选择窗口限制的函数，在显示项任一超过边界时，将不会显示，而是隐藏起来。

windowBitmap
^^^^^^^^^^^^

在窗口显示图片的函数，默认原点为窗口左上角坐标。

windowBitmap(file,dx,dy,dopacity=255)
""""""""""""""""""""""""""""""""""""

直接显示整张图片在窗口上。

windowBitmap(file,dx,dy,rect,dopacity=255)
""""""""""""""""""""""""""""""""""""""""""

这个函数会对显示的图片进行切割， ``rect`` 的前两项是切割原点 ``(sx,sy)`` ，后两项为宽高。

windowBitmapSelect(file,dx,dy,rect,dopacity=255)
""""""""""""""""""""""""""""""""""""""""""""""""

drawArrow(rect,now,page,pagestr="")
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

在窗口绘制箭头的函数， ``rect`` 的前两个为xy坐标， ``rect`` 的宽大于高时为横向箭头，否则为纵向箭头，宽高最高的为箭头的长/宽。

``now`` 为现在的页数， ``page`` 为最大页数。

``pagestr`` 为两箭头中间的文字，如果是纵向箭头，在窗口靠左则文字头朝左，在窗口靠右则文字头朝右。

dispose()
^^^^^^^^^

释放窗口的函数，将会把 ``this`` 从 ``motaGraphics`` 的队列中删除。

GameSelectWindow
~~~~~~~~~~~~~~~~

处理待遇选择项窗口的类，继承自 ``GameWindow`` 。

新增成员
-------

.. csv-table:: 
    :widths: 50, 100

    "index", "当前的选项号，从0开始"
    "rectHeight", "选择矩形的高度"
    "active", "否活跃的标志，为false时将不能通过按键调整选项"
    "items", "选项的集合，为vector<string>类型"
函数
----

构造函数
^^^^^^^^

``GameSelectWindow(wwidth,rectheight,item)`` ：初始化窗口宽度、矩形高度和选择项，窗口的高度将会通过矩形高度进行计算。

refresh()
^^^^^^^^^

自带的 ``refresh`` 函数，会根据当前选项在对应位置绘制矩形，并读取按键调整矩形位置。

drawItem(idx,colour)
^^^^^^^^^^^^^^^^^^^^^

绘制选项内容的函数。

GameGraphics -> motaGraphics
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

用于承载和刷新游戏画面的类，对应跨文件全局变量为 ``motaGraphics`` 。

update(clear_device)
---------------------

用于更新画面的类， ``clear_device`` 用于判断是否在函数开头清空画面，缺省值为 ``true`` ，因为还有 ``ScreenData`` 类会将地图画面显示在屏幕上，那里会清空一次屏幕，所以在 ``MotaMap`` 类中， ``motaGraphics`` 的 ``update`` 是不用清屏的。

dispose()
---------

用于释放画面的类，会清空屏幕和承载的图像和窗口。

