Mota_cpp
=========

**Mota_cpp** 是一个基于C++语言和SFML图形库的魔塔类游戏框架。

主要特点
--------

**画面**

支持640x480的窗口和至多其长宽 **2倍** 大小的分辨率

**音乐**

使用的是SFML的Audio库，目前支持的音乐播放类型为wav, ogg/vorbis和flac。

**编程语言**

基于C++语言和SFML图形库，当前采用的是包含C++20在内的特性，您可以通过IDE调整至最新标准。

.. hint:: 丑话得说在前面，本框架并不防蠢，更不防手贱，比如并不会检查你是否超出范围设置 ``pos=114514`` ，如果你这么玩了，最多只能收获一个error，所以请务必参照标准、规范和指示来做。

目录
--------

.. toctree::
   :maxdepth: 2
   :numbered:
   :caption: 🚀 写在最前

   /front/intro
   /front/config

.. toctree::
   :maxdepth: 2
   :numbered:
   :caption: 💡 外部介绍

   /tool&data/tools
   /tool&data/datas
   /tool&data/projects

.. toctree::
   :maxdepth: 2
   :numbered:
   :caption: 🪄 内部介绍

   /inner/functions
   /inner/classes
   /inner/data_structures

.. toctree::
   :maxdepth: 2
   :numbered:
   :caption: 📚 附录

   /appendix/C++
   /appendix/SFML

