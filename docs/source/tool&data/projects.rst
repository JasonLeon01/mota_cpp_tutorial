工程说明
========

怎样编译工程
~~~~~~~~~~~~

如果您使用的是JetBrains CLion，您可以打开软件并定位至目标项目，找到 ``include\Game\`` 下的所有文件并双击加入顶部栏中，然后将  ``src\`` 下的所有 ``.cpp`` 文件和 ``CMakeLists.txt`` 文件加入顶部栏。

在左侧栏右键点击 ``CmakeLists.txt`` ，选择 :guilabel:`Reload Cmake Project` ，即可加载CMake，右上角点击锤子样式的 :guilabel:`Build` 图标即可。点击绿色右箭头样式的 :guilabel:`Run` 图标可以运行，点击虫子样式的 :guilabel:`Debug` 图标可以调试断点等。

.. hint:: 注意，发布的时候一定要选择Release模式，否则exe会变得特别大！。

发布可运行项目时，需要保留哪些文件
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

您需要保留的有 ``data\`` 、 ``graphics\`` 、 ``sound\`` 和 ``font\`` 及其目录下的所有文件、所有的 ``.dll`` 和 ``.ini`` 文件、 ``main.exe`` 文件，如果您希望玩家可以调整分辨率和音量，也可以保留 ``Config.exe`` 文件。

怎样修改生成exe的图标
~~~~~~~~~~~~~~~~~~~~

您需要首先创建一个自己的 ``Icon.ico`` 文件，放入 ``ico\`` 路径下，在该文件夹下点击 :guilabel:`鼠标右键` ，选择 :guilabel:`在终端中打开` ，输入 ``windres Icon.rc -o Icon.o`` 命令生成对应的 ``Icon.o`` 文件，然后在Clion中 :guilabel:`Reload Cmake Project` 加载CMake，点击 :guilabel:`Build` 构建exe即可。

ShortcutKey.txt是什么，有什么作用？
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

这是用来介绍快捷键的文本，您的工程以后需要用到的快捷键介绍可以写在这里，当状态栏检测到 ``ShortcutKey.txt`` 存在时，会在地图右下角显示一个 ``～Press L～`` ，提示玩家按L键获取快捷键信息。
