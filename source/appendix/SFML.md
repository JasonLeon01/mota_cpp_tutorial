# SFML入门

本节只进行简单的介绍，更多内容请移步 [SFML官方教程](https://www.sfml-dev.org/tutorials/2.6/) 和 [SFML官方文档](https://www.sfml-dev.org/documentation/2.6.0/) 进行了解。

## sf::Color

`Color` 由四个成员组成：`r`, `g`, `b`, `a`，其中 `r`, `g`, `b` 分别代表红、绿、蓝三原色，`a` 代表 alpha 不透明度。

四个成员的范围均为 `0~255`，SFML也提供了如下几个常用的颜色：`Black`, `White`, `Red`, `Green`, `Blue`, `Yellow`, `Magenta`, `Cyan`, `Transparent`。

## sf::Font

SFML的字体类，字体会在 `main.ini` 中进行设置，读取的是 `assets\font\` 文件夹下的字体文件，无需用户安装字体。

## sf::Music & sf::Sound

SFML的音乐类，依托于 `Music` 和 `Sound` 类型变量，`Music` 采用Stream读取而不是在内存中读取，适合用来播放BGM，`Sound` 从内存中读取，适合播放SE。

**音乐的播放需要在Music和Sound类型变量的生命周期内，一旦被销毁将无法播放。**

- `openFromFile(filename)`: 从文件中读取音乐。
- `play()`: 播放加载的音乐。
- `pause()`: 对播放的音乐进行暂停。
- `stop()`: 停止当前播放的音乐。
- `setLoop(loop)`: 设置是否循环播放。
- `setVolume(volume)` & `getVolume()`: 设置和获取当前音量。

## IntRect

用来表示矩形，四个成员分别为x坐标、y坐标、宽度、高度。

## sf::Text

SFML中用于输出文本的类，不过本框架中统合成了 `drawText` 函数。

- `Text(str, font, characterSize)`: Text的构造函数。
- `setString(str)`: 设置文本内容。
- `setCharacterSize(characterSize)`: 设置字号。
- `setLineSpacing(spacingFactor)`: 设置行距。
- `setLetterSpacing(spacingFactor)`: 设置字间距。
- `setStyle(style)`: 设置样式。
- `setOutlineColor(color)`: 设置文本描边颜色。

## sf::Texture

Texture的中文为 **纹理** ，也就是贴在对象/模型上的图片，需要从文件中加载。

- `loadFromFile(string, IntRect)`: 从文件中读取纹理。
- `getSize()`: 获取纹理尺寸。

## sf::Sprite

Sprite的中文为 **精灵** ，承载了纹理并以单个对象的形式呈现在屏幕上。

- `Sprite(texture)`: 初始化Sprite的纹理。
- `setTexture(texture)`: 设置Sprite的纹理。
- `setTextureRect(rectangle)`: 截取纹理中的一部分。
- `setPosition(x, y)`: 设置精灵在屏幕上的位置。
- `setRotation(angle)`: 设置精灵的旋转角度。
- `setScale(factorX, factorY)`: 设置精灵的放大率。
- `setOrigin(x, y)`: 设置精灵的原点坐标。
