内部数据结构
===========

这一部分可以参考 ``include\Game\GameData.h`` 和 ``src\GameData.cpp`` 文件内容。

所有数据的存储是在 :ref:`label_gamedata` 处，可以直接点击跳转。

GameEvent
~~~~~~~~~

用来描述游戏事件的类。

成员
----

.. csv-table::
    :widths: 50, 100

    "ID", "事件编号"
    "name", "事件名称"
    "file", "事件行走图文件名"
    "pos", "在行走图上对应位置"
    "x, y", "xy坐标"
    "triggerCondition", "出现触发判定"
    "move", "是否有动效"
    "through", "是否可穿透"
    "exist", "事件还存在的标志"

函数
----

构造函数
^^^^^^^

GameEvent(ex,ey)，可以初始化对象的初始xy坐标。

order()
^^^^^^^

一切事件指令的存储。

endEvent()
^^^^^^^^^^^

结束本事件的指令。

GameMap
~~~~~~~

用来描述游戏地图数据的类。

成员
----

.. csv-table::
    :widths: 50, 100

    "mapID", "地图所在的ID"
    "mapName", "地图名"
    "bgmFile", "地图BGM的文件名"
    "mapEvents", "地图上事件的集合"

函数
----

haveAnEvent(x,y)
^^^^^^^^^^^^^^^^^

判断地图上(x,y)处是否有事件。

checkEvent(x,y)
^^^^^^^^^^^^^^^^

返回(x,y)处事件的ID。

EcheckEvent(x,y)
^^^^^^^^^^^^^^^^^

直接返回(x,y)处事件的对象。

passible(x,y)
^^^^^^^^^^^^^^

判断(x,y)处是否可通行，判断标准是此处是否有事件，有的话是否可穿透。

GameActors
~~~~~~~~~~

描述游戏角色数据的类。

成员
----

.. csv-table::
    :widths: 30, 100

    "name", "角色名"
    "file", "角色行走图文件名"
    "status", "角色当前状态"
    "level", "角色等级"
    "hp", "角色生命值"
    "atk", "角色攻击力"
    "def", "角色防御力"
    "mdef", "角色魔防"
    "exp", "角色经验"
    "gold", "角色金币"
    "mapID", "角色所在地图编号"
    "x", "角色x坐标"
    "y", "角色y坐标"
    "item", "角色所持有物品数量"


函数
-----

getAtk()
^^^^^^^^^

获取角色攻击的实际值，会减去其衰弱效果值。

getDef()
^^^^^^^^

获取角色防御的实际值，会减去其衰弱效果值。

GamePlayer
~~~~~~~~~~

描述屏幕上玩家数据的类。

成员
----

.. csv-table::
    :widths: 50, 100

    "direction", "方向"
    "step", "角色步数"
    "visible", "是否可见"

函数
----

update()
^^^^^^^^^^

玩家数据的更新，上下左右行走的判断就在于此。

changeSteps()
^^^^^^^^^^^^^^

步数改变时会发生的情况，一般用于阻击、激光、夹击、领域等情况。

GameEnemy
~~~~~~~~~~

描述敌人数据的类。

成员
-----

.. csv-table::
    :widths: 50, 100

    "name", "敌人名字"
    "file", "敌人所在行走图"
    "element", "敌人属性"
    "pos", "敌人所在行走图行数"
    "hp", "敌人生命值"
    "atk", "敌人攻击"
    "def", "敌人防御"
    "conatk", "敌人连击数"
    "exp", "敌人经验值"
    "gold", "敌人金币"
    "animationID", "敌人动画编号"

函数
----

getDamage(aatk=0,adef=0,amdef=0)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

根据当前角色能力数据，获取敌人对自己造成的伤害， ``-1`` 为不可战胜，也可以在参数里面按顺序输入拟定的角色攻击力、防御力和魔防来计算虚拟伤害。

当这些参数为0时，默认为当前角色数值。

getDef()
^^^^^^^^^

获取怪物真实防御，一般用于坚固怪。

getP(p)
^^^^^^^^

判断怪物是否拥有某属性。

getCrisis()
^^^^^^^^^^^

获取怪物临界。

getElement(pid)
^^^^^^^^^^^^^^^^

获取怪物属性及其描述，如果是会变动的属性（如不同的衰弱效果），就将数值写入第二个参数。

GameElement
~~~~~~~~~~~

描述怪物属性数据的类。

成员仅有 ``name`` 和 ``description`` ，描述属性名字和效果。

GameItem
~~~~~~~~~

描述物品数据的类。

.. csv-table::
    :widths: 50, 100

    "name", "物品名字"
    "description", "物品描述"
    "file", "物品所在行走图"
    "pos", "物品所在行走图位置"
    "price", "物品价格"
    "usable", "是否可在物品栏使用"
    "cost", "是否可消耗"

GameAnimation
~~~~~~~~~~~~~~

描述动画数据的类。

.. csv-table::
    :widths: 50, 100

    "pattern", "动画所有图形的队列"
    "SEFile", "动画播放SE的文件名"
    "SETime", "播放SE所在的帧数"

GameNPC
~~~~~~~

描述NPC数据的类。

.. csv-table::
    :widths: 50, 100

    "npcInfo", "对话信息，包含事件ID、对话人名、对话内容"
    "fade", "对话完后是否消失"
    "transName", "对话完后转换成的事件名"
    "directlyFunction", "转换完成是否立刻执行"

.. _label_gamedata:
GameData -> motaData ★★★
~~~~~~~~~~~~~~~~~~~~~~~~~~

一切数据的存储器，所有的数据都存储在这里。

成员
-----

.. csv-table::
    :widths: 50, 100

    "actors", "角色的初始数据存放"
    "animations", "动画数据存放"
    "elements", "属性数据存放"
    "enemies", "敌人数据存放"
    "items", "物品道具数据存放"
    "maps", "地图数据存放"
    "npc", "NPC数据存放"
    "motaName", "用于储存魔塔编号对应的名字"

.. hint:: 其中 ``actors`` 和 ``maps`` 仅仅存放初始数据， **请勿修改** ，关于游戏中相关的在后面。

函数
-----

init()
^^^^^^^

初始化函数，会读取数据库重置上述信息，仅在打开游戏时调用，请勿随意使用。

searchMap(mapnane)
^^^^^^^^^^^^^^^^^^^

按照地图名搜索地图的函数，返回相应地图编号，同名地图返回序号靠前的。

GameTemp -> motaTemp
~~~~~~~~~~~~~~~~~~~~

用来存储临时变量的类，后续自行DIY也可以在此处修改，当前已有的临时变量会在 ``order`` 函数处对其赋值。

.. csv-table:: 当前已有临时变量含义
    :widths: 50, 100

    "battleEnemyID", "当前与之战斗的敌人ID"
    "shopType", "当前触发的商店类型"
    "shopID", "当前商店编号"
    "functionEventID", "正在交互的事件编号"
    "closeMS", "关闭状态栏，也可以使用7号变量控制关闭"
    "transEventName", "事件结束后，更改成的名字，可不填写"
    "directlyFunction", "事件更改名字后，是否直接触发"
    "toDisposeEvent", "是否结束事件"
    "gameOver", "游戏结束的标志"
    "messageInfo", "对话信息"
    "floorEnemies", "记录当前楼层怪物信息"

GameVariables -> motaVariables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

游戏变量相关的集合。

.. csv-table::
    :widths: 50, 100

    "variables", "游戏内部变量，可参考RMXP的开关和变量，具体代表含义在variables.txt处标注"
    "itemRecord", "记录获得过的物品"
    "floorRecord", "记录去过的楼层"
    "eventRecord", "记录消失过的事件"
    "transRecord", "记录变更过名字的事件"

ScreenData -> screenData ★★★★★
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

存储游戏屏幕上所显示数据的类。

成员
-----

.. csv-table::
    :widths: 50, 100

    "actors", "角色数据"
    "player", "玩家数据"
    "visualMap", "当前地图数据"

其中， ``screenData.actors`` 和 ``screenData.visualMap`` 为本类核心。

函数
----

init()
^^^^^^

初始化函数，会将角色数据从 ``motaData.actors`` 中读取。

loadMap(mapID)
^^^^^^^^^^^^^^^

读取地图的函数，会从 ``motaData.maps`` 中读取地图文件数据并根据当前的 ``motaVariables`` 更改地图样式。

mapStatus()
^^^^^^^^^^^

显示游戏状态栏的函数，状态栏的DIY在此处修改。

showMap(gmap,x,y)
^^^^^^^^^^^^^^^^^^^

在画面的(x,y)处显示地图 ``gmap`` 的函数，作用和 ``motaGraphics.update()`` 相当，游戏中的动画也在此处显示，在遍历事件处有地图显示伤害的配置，可在此处自行修改。

此外，还可以在最后插一个 ``float`` 类型的变量 ``rate`` ，代表地图的放缩率，缺省值为1。

waitCount(times)
^^^^^^^^^^^^^^^^

等待的函数，等待的帧数期间不可操作。

addAnimation(id,x,y) & addEVAnimation(id,x,y)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

在地图上显示动画的函数，前者的xy为屏幕坐标，后者的xy为地图坐标（0~10）

loadData(fileid) & saveData(fileid)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

顾名思义，读档和存档的函数，具体的DIY在此处修改，因为C++没有序列化数据的能力，所以大多都要拆散自行存储。
