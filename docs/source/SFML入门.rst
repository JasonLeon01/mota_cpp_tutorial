SFML入门
=========

本节只进行简单的介绍，更多内容请移步 https://www.sfml-dev.org/tutorials/2.6/ 和 https://www.sfml-dev.org/documentation/2.6.0/ 进行了解。

1. sf::Color
~~~~~~~~~~~~

``Color`` 由四个成员组成， ``r`` , ``g`` , ``b`` , ``a`` ，其中 ``r`` , ``g`` , ``b`` 就是红绿蓝三原色， ``a`` 是 ``alpha`` 不透明度。

四个成员的范围均为 ``0~255`` ， ``sf::Color`` 也准备了如下几个常用的颜色：

``Black`` , ``White`` , ``Red`` , ``Green`` , ``Blue`` , ``Yellow`` , ``Magenta`` , ``Cyan`` , ``Transparent`` 。

2. sf::Font
~~~~~~~~~~~

为SFML的字体类，字体会在 ``main.ini`` 中进行设置，读取的是 ``font\`` 文件夹下的字体文件，无需用户安装字体。

3. sf::Music & sf::Sound
~~~~~~~~~~~~~~~~~~~~~~~~~

都是SFML的音乐类，依托于 ``Music`` 和 ``Sound`` 类型变量， ``Music`` 采用Stream读取而不是在内存中读取，适合用来播放BGM， ``Sound`` 从内存中读取，适合播放SE。

.. hint:: 音乐的播放需要在Music和Sound类型变量的生命周期内，一旦被销毁将无法播放。

3.1. openFromFile(filename)
---------------------------

从文件中读取，完整的为 ``bool sf::Music::openFromFile(const std::string& filename)`` 。

3.2. play()
-----------

播放加载的音乐。

3.3. pause()
-----------

对播放的音乐进行暂停。

3.4. stop()
----------

停止当前播放的音乐。

3.5. setLoop(loop)
------------------

设置是否循环播放，参数填写 ``true`` 或者 ``false`` 。

3.6. setVolume(volume) & getVolume()
----------------------------------

设置和获取当前音量，注意，音量值为 ``float`` 类型。

4. IntRect
~~~~~~~~~~

用来表示矩形，四个成为分别为x坐标、y坐标、宽、高。

5. sf::Text
~~~~~~~~~~~

SFML中用于输出文本的类，不过本框架中统合成了 ``drawText`` 函数，不过原理您可以通过下面了解。

5.1. Text(str, font, characterSize)
------------------------------------

为Text的构造函数，三个参数分别为文本、字体和字号。

5.2. setString(str)
---------------------

为Text设置文本的函数。

5.3. setCharacterSize(characterSize)
-----------------------------------

为Text设置字号的函数。

5.4. setLineSpacing(spacingFactor)
---------------------------------

为Text设置行距的函数。

5.5. setLetterSpacing(spacingFactor)
-----------------------------------

为Text设置字之间间隔的函数。

5.6. setStyle(style)
--------------------

为Text设置样式的函数，如粗体、斜体等。

5.7. setOutlineColor(color)
---------------------------

为Text设置颜色的函数。

6. sf::Texture
~~~~~~~~~~~~~~~

Texture的中文为 **纹理** ，也就是贴在对象/模型上的图片，需要从文件中加载，本框架会将 ``graphics\`` 默认的三个文件夹的所有内容都预先加载到 ``motaSystem.textureCache`` 中，直接调用即可，如果想要了解原理可以参考如下内容。

6.1. loadFromFile(string, IntRect)
---------------------------------

从文件中读取纹理， ``IntRect`` 会限制读取范围，如果不设置会默认全部读取。

6.2. getSize()
-------------

会获取这个纹理的尺寸。

7. sf::Sprite
~~~~~~~~~~~~~~

Sprite的中文为 **精灵** ，承载了纹理并以单个对象的形式呈现在屏幕上。本框架将纹理、精灵和屏幕绘制均进行了封装，如果对原理感兴趣的话可以了解以下内容：

7.1 Sprite(texture)
--------------------

用于初始化Sprite的纹理。

7.2. setTexture(texture)
-------------------------

用于设置Sprite的纹理。

7.3. setTextureRect(rectangle)
-------------------------------

用于截取纹理中的一部分。

7.4. setPosition(x, y) & getPosition()
---------------------------------------

用于设置和获取精灵在屏幕上的位置。

7.5. setRotation(angle) & getRotation()
---------------------------------------

用于设置和获取精灵的旋转角度。

7.6. setScale(factorX, factorY) & getScale()
---------------------------------------------

用于设置和获取精灵的放大率，反复使用 ``setScale`` 会将放大率相乘。

7.7. setOrigin(x, y) & getOrigin()
-----------------------------------

用于设置和获取精灵的原点坐标。
