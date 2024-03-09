# 内部类

这一部分可以参考 `include\Game\System\` 和 `src\System\` 文件夹内容。

## SceneBase

场景类型的基类，所有的场景类都要是这个的子类。

## GameImage

Image类型，可以类比为RMXP里的 `Sprite` 类（虽然这个就是SFML的 `Sprite` 的封装）。

### 成员

| x,y,z | Image的xy坐标，z是优先级，z越高在屏幕上的优先级越大，显示的就越靠前 |
|-------|-------------------------------------------------------------------------|
| origin_x,origin_y | 设置图像的原点，默认都为0 |
| scale_x,scale_y | 设置图像在横纵方向上的放缩率 |
| width,height,sx,sy | 用于切割图像的参数，默认都为0，width和height为0时不进行切割 |
| opacity | 图像的不透明度 |
| angle | 图像的旋转角度 |
| visible | 决定一个图像是否可见 |

### 函数

#### 构造函数

GameImage(file,x=0,y=0,width=0,height=0,sx=0,sy=0)

对相应的成员进行初始化。除了 `file` 以外，其余参数缺省值为0。

#### setSprite

setSprite(file,x=0,y=0,width=0,height=0,sx=0,sy=0)

对 `Image` 的 `Sprite` 进行设置（更新），除了 `file` 以外，其余参数缺省值为0。

#### show

展示 `Image` 到屏幕上，一般用于 `GameGraphics` 中。

## GameText

Text类型，封装了SFML的 `Text` 类。

### 成员

x,y,z: Text的xy坐标，z是优先级，z越高在屏幕上的优先级越大，显示的就越靠前。

### 函数

#### 构造函数

GameText(*font,txt,x=0,y=0)

#### setText

setText(*font,txt,x=0,y=0)

设置 `Text` 的属性， `font` 需要引用，因为这个结构非常大，不引用会导致次次复制，传递非常慢。

#### getSize

获取 `Text` 的宽高。

#### show

展示 `Text` 到屏幕上，一般用于 `GameGraphics` 中。

## System -> motaSystem

本框架的系统类，用于储存各种和系统相关的数据，对应跨文件全局变量为 `motaSystem` 。

### 成员

| window | 为这个游戏的窗口，一切和窗口相关的都在这里调整 |
|--------|-------------------------------------------------|
| font | 为游戏字体，字体的设置在这里调整 |
| bgm | 为游戏BGM，Music类型变量 |
| scene | 为设置场景的变量，motaSystem.scene可以类比为RMXP里的$scene |
| gameTime | 用以计算游戏时间，每刷新一次将会+1，一般是用来设置行走动画相关的内容 |
| resolutionRatio | 是当前的分辨率（放大率），最低是1（即默认的640x480），最大是2（变成1280x960） |
| frameRate | 帧率，目前支持15-60帧 |
| windowOpacity | 是默认的窗口不透明度 |
| BGMVolume,SEVolume | 为BGM和SE的音量。 |
| xxSE | 为各种SE的文件名，具体参考main.ini |
| textureCache | 为纹理的缓存，这些都会在开始游戏时缓存完成。 |

### 函数

#### init

对各种成员初始化的函数，读取系统数据也是在这里进行。

#### bgmSwitch

切换BGM使用的函数。

## Input

关于输入响应的命名空间，和魔塔样板的全键盘脚本相似。

### 函数

#### press

原理是 `GetAsyncKeyState(key)&0x8000` ，返回值看是否非零，这样可以无缓存的判定键盘的按下，只要你的手指还按在键盘上，就会不断地响应。

#### repeat

原理是用计数器统计 `GetAsyncKeyState(key) & 0x8000` 相应的次数，若干次内将不再返回 `true` 。

#### trigger

原理是在按下时记录一次，松开时取消记录，而有记录时将不再相应，按一次键盘无论按多久都只响应一次。

#### pressConfirm(), repeatConfirm(), triggerConfirm()

使用上述三种按法按下确认键，将会返回 `true` 。确认键指的是 `Enter` 和 `Space` 。

#### pressCancel(), repeatCancel(), triggerCancel()

使用上述三种按法按下取消键，将会返回 `true` 。取消键指的是 `X` 和 `Esc` 。

#### doubleClick

判定是否双击的函数，原理是判定两次连续的敲击键盘，如果时间间隔不超过若干毫秒，则判定为双击。

#### dir4

判定四方向的函数，下左右上分别返回0,1,2,3，否则返回-1。

#### leftClick

判定鼠标左键是否按下的函数。

#### rightClick

判定鼠标右键是否按下的函数。

#### scrollUp

判定鼠标滚轮向上滚动的函数。

#### scrollDown

判定鼠标滚轮向下滚动的函数。

## GameWindows

游戏窗口的类，同理，也是在构造函数之后将 `this` 添加到 `motaGraphics` 当中。

### 成员

| x,y,z,width,height,opacity,visible | 窗口的x,y,z坐标、宽高和不透明度以及是否可见。 |
|-------------------------------------|----------------------------------------------|
| haveFunction | 判断是否有执行函数的变量，如果为 `true` ，则在显示的时候会执行里面的 `refresh()` 函数。 |

### 函数

#### 构造函数

GameWindow(rect,wopacity)

#### drawRect

在窗口绘制选择矩形框的函数。

#### drawWText

在窗口里面显示文字的函数，默认原点为窗口左上角坐标。

#### drawWText(dx,dy,content,size=20L,bond=false,color=Color::White)

这是最简单的类型，只有 `dx` 和 `dy` 的设置，直接显示在屏幕上。

#### drawWText(rect,content,pos=0,size=20L,bond=false,color=Color::White)

为描绘文字添加了矩形限制 `rect` ，按照 `pos` 给定的位置来描绘， `pos=0` 时在矩形的左上角， `pos=1` 时在矩形的 **正中央** ， `pos=2` 时在矩形的右上角。

#### drawWTextRotate

为文本描绘提供旋转角度的函数， `angle` 是文字旋转角度。

#### drawWTextSelect

为文本描绘添加选择窗口限制的函数，在显示项任一超过边界时，将不会显示，而是隐藏起来。

#### windowBitmap

在窗口显示图片的函数，默认原点为窗口左上角坐标。

## GameSelectWindow

处理待遇选择项窗口的类，继承自 `GameWindow` 。

### 新增成员

| index | 当前的选项号，从0开始 |
|-------|-----------------------|
| rectHeight | 选择矩形的高度 |
| active | 是否活跃的标志，为false时将不能通过按键调整选项 |
| items | 选项的集合，为vector<string>类型 |

### 函数

#### 构造函数

`GameSelectWindow(wwidth,rectheight,item)` ：初始化窗口宽度、矩形高度和选择项，窗口的高度将会通过矩形高度进行计算。

#### refresh

自带的 `refresh` 函数，会根据当前选项在对应位置绘制矩形，并读取按键调整矩形位置。

#### drawItem

绘制选项内容的函数。

## GameGraphics -> motaGraphics

用于承载和刷新游戏画面的类，对应跨文件全局变量为 `motaGraphics` 。

### 函数

#### update

用于更新画面的类， `clear_device` 用于判断是否在函数开头清空画面，缺省值为 `true` ，因为还有 `ScreenData` 类会将地图画面显示在屏幕上，那里会清空一次屏幕，所以在 `MotaMap` 类中， `motaGraphics` 的 `update` 是不用清屏的。

#### dispose

用于释放画面的类，会清空屏幕和承载的图像和窗口。
