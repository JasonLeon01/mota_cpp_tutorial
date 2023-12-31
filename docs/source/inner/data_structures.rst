内部数据结构
===========

这一部分可以参考 ``include\Game\Data`` 和 ``src\Data`` 文件夹内容。

所有数据的存储是在 :ref:`label_gamedata` 处，可以直接点击跳转。

Object
~~~~~~

用来描述游戏事件的类，和SFML自带的Event类区分。

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

Object(name,x=0,y=0)

可以初始化对象的事件名和初始xy坐标。

Map
~~~~

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

getLineEvents(pair, pair)
^^^^^^^^^^^^^^^^^^^^^^^^^

获取在两个同x或同y坐标之间的事件列表。

Actor
~~~~~

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

Enemy
~~~~~~

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

getDamage(\*actor,\*elements)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

根据当前角色能力数据，获取敌人对自己造成的伤害， ``-1`` 为不可战胜，也可以在参数里面按顺序输入拟定的角色攻击力、防御力和魔防来计算虚拟伤害。

getDef(\*actor)
^^^^^^^^^^^^^^^^

获取怪物真实防御，一般用于坚固怪。

getP(p)
^^^^^^^^

判断怪物是否拥有某属性。

getCrisis(\*actor)
^^^^^^^^^^^

获取怪物临界。

getElement(pid)
^^^^^^^^^^^^^^^^

获取怪物属性及其描述，如果是会变动的属性（如不同的衰弱效果），就将数值写入第二个参数。

Element
~~~~~~~

描述怪物属性数据的类。

成员仅有 ``name`` 和 ``description`` ，描述属性名字和效果。

Item
~~~~~

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

Animation
~~~~~~~~~~

描述动画数据的类。

.. csv-table::
    :widths: 50, 100

    "pattern", "动画所有图形的队列"
    "SEFile", "动画播放SE的文件名"
    "SETime", "播放SE所在的帧数"

NPC
~~~

描述NPC数据的类。

.. csv-table::
    :widths: 50, 100

    "npcInfo", "对话信息，包含事件ID、对话人名、对话内容"
    "fade", "对话完后是否消失"
    "transName", "对话完后转换成的事件名"
    "directlyFunction", "转换完成是否立刻执行"

.. _label_gamedata:
Data -> motaData ★★★
~~~~~~~~~~~~~~~~~~~~~~

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

Temp -> motaTemp
~~~~~~~~~~~~~~~~

用来存储临时变量的类，后续自行DIY也可以在此处修改。

.. csv-table:: 当前已有临时变量含义
    :widths: 50, 100

    "battleEnemyID", "当前与之战斗的敌人ID"
    "shopType", "当前触发的商店类型"
    "functionEventID", "正在交互的事件编号"
    "closeMS", "关闭状态栏，也可以使用7号变量控制关闭"
    "transEventName", "事件结束后，更改成的名字，可不填写"
    "directlyFunction", "事件更改名字后，是否直接触发"
    "toDisposeEvent", "是否结束事件"
    "gameOver", "游戏结束的标志"
    "ending", "结局的标志"
    "nextMove", "事件进一步指令的标志，维持事件执行先后顺序"
    "addPower", "记录商店增加能力的数值"
    "initPrice", "记录商店价格"
    "messageInfo", "对话信息"
    "floorEnemies", "记录当前楼层怪物信息"

Variables -> motaVariables
~~~~~~~~~~~~~~~~~~~~~~~~~~

游戏变量相关的集合。

成员
----

.. csv-table::
    :widths: 50, 100

    "variables", "游戏内部变量，可参考RMXP的开关和变量，具体代表含义在variables.txt处标注"
    "itemRecord", "记录获得过的物品"
    "floorRecord", "记录去过的楼层"
    "eventRecord", "记录消失过的事件"
    "transRecord", "记录变更过名字的事件"

函数
----

init()
^^^^^^^

初始化函数，会读取数据库重置上述信息，仅在打开游戏和读档时调用，请勿随意使用。

Interpreter
~~~~~~~~~~~

游戏事件解释器，用来解释游戏事件名并执行对应操作的类。

成员
----

构造函数
^^^^^^^^^

Interpreter(eventName)

将事件名告知类对象。

execute(\*obj=nullptr)
^^^^^^^^^^^^^^^^^^^^^^

执行事件的函数，参数为事件对象，如果不填写则视为临时事件，部分指令不会造成影响。

replaceToVar(source)
^^^^^^^^^^^^^^^^^^^^

可以将形如 ``[x]`` 的字符串替换为对应序号变量的值，为静态函数，可以使用 ``Interpreter::replaceToVar(source)`` 调用。

initDialogue(source)
^^^^^^^^^^^^^^^^^^^^

初始化对话的函数，参数为对话内容，会将对话内容中的 ``[x]`` 替换为对应序号变量的值，以及将 ``[hp]`` 等替换为主角当前能力值。

Player
~~~~~~

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

triggerEvent(\*obj)
^^^^^^^^^^^^^^^^^^^

触发事件的函数，参数为事件对象，如果不填写则视为临时事件，部分指令不会造成影响。

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

loadMap(mapID,*gmap)
^^^^^^^^^^^^^^^

读取地图的函数， ``gmap`` 一般来说是 ``screenData.visualMap`` ，不过你也可以从 ``motaData.maps`` 中读取其他地图文件数据并用这个函数读取。

mapStatus()
^^^^^^^^^^^

显示游戏状态栏的函数，状态栏的DIY在此处修改。

showMap(gmap,x,y,rate=1.f,visible=true,clear_device=true)
^^^^^^^^^^^^^^^^^^^

在画面的(x,y)处显示地图 ``gmap`` 的函数，作用和 ``motaGraphics.update()`` 相当，游戏中的动画也在此处显示，在遍历事件处有地图显示伤害的配置，可在此处自行修改。

``rate`` 代表地图的放缩率。

``visible`` 代表在地图上主角的行走图是否可见。

``clear_device`` 代表是否清空画面，如果是新开一个窗口预览地图，需要设置为 ``false`` 。

waitCount(times)
^^^^^^^^^^^^^^^^

等待的函数，等待的帧数期间不可操作。

addAnimation(id,x,y) & addEVAnimation(id,x,y)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

在地图上显示动画的函数，前者的xy为屏幕坐标，后者的xy为地图坐标（0~10）

loadData(fileid) & saveData(fileid)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

顾名思义，读档和存档的函数，具体的DIY在此处修改。

11.29的更新将json引入了本框架，以后将可以使用json对数据序列化。

doOrder(lists,\*obj)
^^^^^^^^^^^^^^^^^^^^^

执行一个事件串，一般用于对话中。

transition1(time=10)
^^^^^^^^^^^^^^^^^^^^^

淡入的函数，参数为淡入的帧数。

transition2(time=10)
^^^^^^^^^^^^^^^^^^^^^

淡出的函数，参数为淡出的帧数。

dispose()
^^^^^^^^^^

清空屏幕的函数，一般用于切换场景。
