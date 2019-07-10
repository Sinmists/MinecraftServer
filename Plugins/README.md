# Minecraft Server Plugins

**这是什么文档？**

目前计划长期运行的Minecraft服务器建设中，插件配置需要频繁翻阅不同文档，且每个插件常用的只有部分文档内容而已，因此按照个人需求整合并修改了部分配置内容，可参考但不建议全盘搬运，因此造成的后果概不负责，请以官方文档为准。

正弦默示录  
2019.7.9

---

**目录**
[TOC]

---

## [权限管理]LuckPerms

[Github](https://github.com/PluginsCDTribe/LuckPerms) | 
[中文Wiki](https://pluginscdtribe.github.io/wiki/luckperms/)

### 安装

#### 初始化

1. 根据平台下载`LuckPerms-???-x.x.x.jar` 文件。[最新版本](https://luckperms.github.io/)
2. 把 LuckPerms 的 jar 文件放入Mod 或插件所在路径，路径通常是 `/plugins/` ，或者 `/mods/`
3. 完全关闭服务器，然后再打开，生成默认配置，配置文件在 `/plugins/LuckPerms/config.yml` 或 `/config/luckperms/luckperms.conf`。

#### 环境需求

Java 8

#### 切换存储类型

**请记住如果你想改变存储类型的话，你的数据不会自动转移到新的存储库中。**
LuckPerms 插件所使用的默认数据存储类型是 H2 数据库。
所有的数据都会储存在LuckPerms插件目录下的 `luckperms.db.mv.db` 文件之中。

存储类型选项可以在 `config.yml` 或 `luckperms.conf` 文件中修改。
```
# Which storage method the plugin should use.
# Currently supported: mysql, postgresql, sqlite, h2, json, yaml, mongodb
# Fill out connection info below if you're using MySQL, PostgreSQL or MongoDB
storage-method: h2
```

**H2 / SQLite**

默认的存储类型是 H2，这两种文件类型都是以SQL数据库为基础的。

如果你选择使用 H2 数据库的话（默认设置），所有的数据都会储存在 `luckperms-h2.mv.db` 文件中。
SQLite 类型所提供的存储文件是 `luckperms-sqlite.db`。

要想使用这两种类型中的一种，请将配置设置为：
```
storage-method: h2
# or
storage-method: sqlite
```

### 命令

#### 初始化

**给予修改权限的全部权限**

第一件事情就是给你LP的所有权限。
当LP首次安装后，没有人能够使用插件的任何命令。

在服务器控制台输入以下命令，注意修改用户名
`/lp user Sinmists permission set luckperms.* true` 

**创建第一个权限组owner**

`/luckperms creategroup owner`
>注意LP默认存在一个default的权限组，玩家默认处于该组内，建议新建player权限组并执行`/lp group default parent set player`，并将需要给默认玩家的权限赋予player组中，因为默认玩家信息并不存储于default组中，所以需要给与特殊权限时建议新建其他权限组

**给与owner组原版MC所有权限**

`/luckperms group owner permission set minecraft.command.* true` 

**将玩家加入到owner权限组中**

 `/luckperms user Sinmists parent add owner` 

#### 概览

**= 参数键值：**
<必需> - 运行指令时你 **必需** 指定这个参数
[可选] - 如果没有指定将会使用默认
如果你想在参数中添加空格，必须像这样 " " 使用引号把参数包起来

#### 常用命令解析

>此处只列出常用命令，全部命令请查看官网文档

##### 基础
```
/lp
权限: n/a
基础的 LuckPerms 命令。将会打印用户有权限使用的所有的命令，包含每个命令的基础信息，和接受的参数。

/lp info
权限: luckperms.info
列出 LuckPerms 的一些信息/数据，包括 debug 输出，统计，设置和配置中的一些重要的值。

/lp bulkupdate
权限: 仅控制台
允许你执行一次对所有权限数据的更新，慎用，参照文档。

/lp import <file>
权限: luckperms.import
参数:
<file> - 导入的文件
从文件导入 LuckPerms 的数据，文件必须是一列命令，以 "/luckperms" 开头，这个文件可以使用 export 命令生成，文件必须在插件的目录下。

/lp export <file>
权限: luckperms.export
参数:
<file> - 导出的文件
将 LuckPerms 的数据导出到一个文件，这个文件也可以作为一个备份，或者在 LuckPerms 的安装之间转移数据。这个文件可以使用 import 命令重新导入，生成的文件在插件的目录下。

/lp creategroup <group>
权限: luckperms.creategroup
参数:
<name> - 组的名称
创建一个新的组。

/lp deletegroup <group>
权限: luckperms.deletegroup
参数:
<name> - 组的名称
永久的删除一个组。

/lp listgroups
权限: luckperms.listgroups
显示当前的所有的组。

/lp createtrack
权限: luckperms.createtrack
参数:
<name> - 路线名称
创建新的路线。

/lp deletetrack
权限: luckperms.deletetrack
参数:
<name> - 路线的名称
永久删除一个路线。


/lp listtracks
权限: luckperms.listtracks
显示当前所有的路线。
```

##### 用户 (/lp user <user> ...)
```
/lp user <user> info
权限: luckperms.user.info
显示一个用户的信息，包括用户名，主组，继承组，和当前的上下文。

/lp user <user> permission
参见 权限 (/lp user <user> permission ... | /lp group <group> permission ...)

/lp user <user> parent
参见 权限 (/lp user <user> permission ... | /lp group <group> permission ...)

/lp user <user> editor
权限: luckperms.user.editor
开启编辑指定的用户的权限的网页接口，当更改保存后，你将会收到一条命令，使用后使更改生效。

/lp user <user> promote <track> [context...]
权限: luckperms.user.promote
参数:
<track> - 升级遵循的路线 * [context...] - 升级使用的上下文
这个命令将会沿着一条路线提升玩家，命令会检查玩家在给出的上下文里是否在这个路线上，如果用户没有在这条路线，他们将会被加入这条路线的第一个组，如果玩家在这条路线上的不止一个组，命令将会执行失败。在其他情况下，玩家将会被成功提升，并将会被从现有的组移除。如果路线动作影响了用户的主组，他们也会被更新。

/lp user <user> demote <track> [context...]
权限: luckperms.user.demote
参数:
<track> - 降级的遵循的路线 * [context...] - 降级使用的上下文
这个命令将会沿着一条路线降级玩家，命令会检查玩家在给出的上下文里是否在这个路线上，如果用户没有在这条路线，或者玩家在这条路线上的不止一个组，命令将会执行失败。在其他情况下，玩家将会被成功降级，并将会被从现有的组移除。如果路线动作影响了用户的主组，他们也会被更新。

/lp user <user> showtracks
权限: luckperms.user.showtracks
显示玩家当前所在的全部路线。

/lp user <user> clear [context...]
权限: luckperms.user.clear
参数:
[context...] - 用于过滤的上下文
清除玩家的权限，继承组和元数据。
```

##### 组 (/lp group <group> ...)
```
/lp group <group> info
权限: luckperms.group.info
显示一个组的信息。

/lp group <group> permission
参见 权限 (/lp user <user> permission ... | /lp group <group> permission ...)

/lp group <group> parent
参见 权限 (/lp user <user> permission ... | /lp group <group> permission ...)

/lp group <group> editor
权限: luckperms.group.editor
开启编辑指定的组的权限的网页接口，当更改保存后，你将会收到一条命令，使用后使更改生效。

/lp group <group> listmembers [page]
权限: luckperms.group.listmembers
参数:
[page] - 查看的页数
显示直接继承这个组的用户/组

/lp group <group> setweight <weight>
权限: luckperms.group.setweight
参数:
<weight> - 设置的权重
设置组的权重值，这决定了决定用户的权限的顺序。越大的值代表越高的权重。

/lp group <group> showtracks
权限: luckperms.group.showtracks
显示一个组所在的所有的路线。

/lp group <group> clear [context...]
权限: luckperms.group.clear
参数:
[context...] - 用于过滤的上下文
清除组的权限，继承组和元数据。

/lp group <group> rename <new name>
/lp group <group> rename
权限: luckperms.group.rename
参数:
<new name> - 组的新的名称
更改组的名称，注意任何组的成员都不会知道这个变更，他们还将在原来的旧组的组名。如果你希望更新这些状态，你应该在控制台使用/lp bulkupdate来更新存在的条目。

/lp group <group> clone <name of clone>
/lp group <group> clone
权限: luckperms.group.clone
参数:
<new name> - 复制的名称
创建一个组的不同名称的拷贝。
```

##### 权限 (/lp user <user> permission ... | /lp group <group> permission ...)
```
info
权限: luckperms.user.permission.info 或 luckperms.group.permission.info
显示一个用户/组拥有的所有的权限。

set <node> <true/false> [context...]
权限: luckperms.user.permission.set or luckperms.group.permission.set
参数:
<node> - 设置的权限节点 * <true|false> - 设置权限的值 * [context...] - 设置权限的上下文
设置（或给予）某个用户/组一个权限，提供 false 值将会否定这个权限。

unset <node> [context...]
权限: luckperms.user.permission.unset or luckperms.group.permission.unset
参数:
<node> - 取消设置的权限节点 * [context...] - 取消设置权限的上下文
取消设置一个用户或组的权限节点。

settemp <node> <true/false> <duration> [context...]
权限: luckperms.user.permission.settemp or luckperms.group.permission.settemp
参数:
<node> - 设置的权限节点 * <true|false> - 设置的权限的值 * <duration> - 权限过期的时间 * [context...] - 权限设置的上下文
给一个玩家/组设置临时权限，提供 false 值将会否定这个权限。持续时间应为时间段或者一个标准的 Unix 时间戳，比如 "3d13h45m" 将会设置权限在 3 天, 13 小时 45 分钟后过期。"1482694200" 会设置过期时间为 7:30PM 于 25th December 2016。

unsettemp <node> [context...]
权限: luckperms.user.permission.unsettemp or luckperms.group.permission.unsettemp
参数:
<node> - 取消设置的权限节点 * [context...] - 取消设置权限的上下文
取消设置一个用户或组的临时权限节点。

check <node> [context...]
权限: luckperms.user.permission.check or luckperms.group.permission.check
参数:
<node> - 检查的权限节点 * [context...] - 检查的权限节点的上下文
检查一个组或者玩家有特定的权限

checkinherits <node> [context...]
权限: luckperms.user.permission.checkinherits or luckperms.group.permission.checkinherits
参数:
<node> - 检查的权限节点 * [context...] - 检查的权限节点的上下文
检查一个组或者玩家继承了特定的权限，如果是，从哪里继承的。
```

##### 继承 (/lp user <user> parent ... | /lp group <group> parent ...)
```
info
权限: luckperms.user.parent.info or luckperms.group.parent.info
显示一个用户/组的继承的组

set <group> [context...]
权限: luckperms.user.parent.set or luckperms.group.parent.set
参数:
<group> - 设置的组 * [context...] - 设置的组的上下文
设置一个用户/组的继承组，不像是 "parent add" 命令，这个命令将会清空所有已经存在的组。"add" 命令只会简单的将组添加到已经存在的组里，如果命令执行时没有上下文环境，这个插件也会更新玩家的主组。

add <group> [context...]
权限: luckperms.user.parent.add or luckperms.group.parent.add
参数:
<group> - 添加的组 * [context...] - 添加的组用的上下文
添加一个集成组到一个玩家/组，不像是 "parent set" 命令，这个命令只会将组添加进已经存在的组的列表。没有已经存在的继承组会被移除，用户的主组也不会被影响。

remove <group> [context...]
权限: luckperms.user.parent.remove or luckperms.group.parent.remove
参数:
<group> - 移除的组 * [context...] - 移除的组的上下文
移除一个用户/组的继承组。

settrack <track> <group> [context...]
权限: luckperms.user.parent.settrack or luckperms.group.parent.settrack
参数:
<track> - 设置的路线 * <group> - 设置的组，或者这个路线的组的相对位置 * [context...] - 设置的组的上下文
设置用户/组在给出的路线的位置，这个跟 set 命令相同，除了这个将会清除在指定的路线上已经存在的组，其他继承组不会被影响。

addtemp <group> <duration> [context...]
权限: luckperms.user.parent.addtemp or luckperms.group.parent.addtemp
参数:
<group> - 添加的组 * <duration> - 组的过期时间 * [context...] - 添加组的上下文
给一个玩家/组添加临时继承组。持续时间应为时间段或者一个标准的 Unix 时间戳，比如 "3d13h45m" 将会设置权限在 3 天, 13 小时 45 分钟后过期。"1482694200" 会设置过期时间为 7:30PM 于 25th December 2016。

removetemp <group> [context...]
权限: luckperms.user.parent.removetemp or luckperms.group.parent.removetemp
参数:
<group> - 移除的组 * [context...] - 移除的组的上下文
移除一个用户/组的临时继承组。

clear [context...]
权限: luckperms.user.parent.clear or luckperms.group.parent.clear
参数:
[context...] - 用于过滤的上下文
移除所有继承组。

cleartrack <track> [context...]
权限: luckperms.user.parent.cleartrack or luckperms.group.parent.cleartrack
参数:
<track> - 移除的路线 * [context...] - 用于过滤的上下文
移除指定路线的玩家/组的所有继承组。
```
---

## [基础指令]Nucleus

[Github](https://ore.spongepowered.org/Nucleus/Nucleus) | 
[中文文档](http://www.mcbbs.net/thread-787354-1-1.html)

### 安装

下载[最新版本]()放入Mod文件夹即可

### 命令

#### admin模块
模块|命令|基础权限|命令描述
-|-|-|-
admin|/blockzap|nucleus.blockzap.base|把目标方块设置成空气。
admin|/broadcast|nucleus.broadcast.base|向整个服务器广播消息。
admin|/exp|nucleus.exp.base|显示特定玩家所拥有的经验。
admin|/exp give|nucleus.exp.give.base|向特定玩家给予一定经验。
admin|/exp set|nucleus.exp.set.base|设置特定玩家所调用的经验。
admin|/exp take|nucleus.exp.take.base|把经验从特定玩家身上移除。
admin|/gamemode|nucleus.gamemode.base|设置一个玩家的游戏模式。
admin|/kill|nucleus.kill.base|杀死特定的玩家和/或实体。
admin|/killentity|nucleus.killentity.base|杀死特定的实体。
admin|/plainbroadcast|nucleus.plainbroadcast.base|允许在广播消息时不带前缀或后缀。
admin|/stop|nucleus.stop.base|停止服务器。
admin|/sudo|nucleus.sudo.base|强制玩家执行一个命令。
admin|/tellplain|nucleus.tellplain.base|允许在向玩家发送消息时不带前缀或后缀。

#### afk模块
模块|命令|基础权限|命令描述
-|-|-|-
afk|/afk|nucleus.afk.base|切换玩家的AFK状态。
afk|/afkrefresh|nucleus.afkrefresh.base|停用所有AFK缓存。

#### back模块
模块|命令|基础权限|命令描述
-|-|-|-
back|/back|nucleus.back.base|允许玩家回到上一次死亡或传送点的位置。

#### ban模块
模块|命令|基础权限|命令描述
-|-|-|-
ban|/ban|nucleus.ban.base|封禁玩家。
ban|/checkban|nucleus.checkban.base|查看玩家是否已被封禁。
ban|/tempban|nucleus.tempban.base|临时封禁一个玩家。
ban|/unban|nucleus.unban.base|为玩家解除封禁。

#### chat模块
模块|命令|基础权限|命令描述
-|-|-|-
chat|/me|nucleus.me.base|执行下一条聊天信息，具体执行方式由服务端设置。

#### command-spy模块
模块|命令|基础权限|命令描述
-|-|-|-
command-spy|/commandspy|nucleus.commandspy.base|启用或禁用对他人输入指令内容的窥视。

#### core模块
模块|命令|基础权限|命令描述
-|-|-|-
core|/nucleus|nucleus.nucleus.base|显示Nucleus的版本和模块信息。
core|/nucleus clearcache|nucleus.nucleus.clearcache.base|清除在Nucleus中缓存的玩家信息，并强迫Nucleus在玩家登陆时从文件读取。
core|/nucleus debug|nucleus.nucleus.debug.base|用于定义服务器问题的实用工具。
core|/nucleus debug getuuids|nucleus.nucleus.debug.getuuids.base|获取和特定玩家名关联的所有UUID。
core|/nucleus debug refreshuniquevisitors|nucleus.nucleus.debug.refreshuniquevisitors.base|在{{uniquecount}}不同步时刷新。
core|/nucleus debug setsession|nucleus.nucleus.debug.setsession.base|启用或禁用调试模式，如果配置文件中有所设置，该配置会将其覆盖。
core|/nucleus getuser|nucleus.getuser.base|获取或刷新玩家对应的记录。
core|/nucleus info|nucleus.nucleus.info.base|在服务端根目录创建一个包含有服务端本身和Nucleus的信息的文件。
core|/nucleus itemalias|nucleus.nucleus.itemalias.base|管理物品别名。。
core|/nucleus itemalias clear|nucleus.nucleus.itemalias.clear.base|移除一个物品的所有别名。
core|/nucleus itemalias remove|nucleus.nucleus.itemalias.remove.base|移除一个物品的一个别名。
core|/nucleus itemalias set|nucleus.nucleus.itemalias.set.base|设置一个物品的别名。
core|/nucleus printperms|nucleus.nucleus.printperms.base|输出用于Nucleus的所有权限的列表。
core|/nucleus rebuildusercache|nucleus.nucleus.rebuildusercache.base|重建Nucleus的玩家缓存。
core|/nucleus reload|nucleus.nucleus.reload.base|重新加载Nucleus的配置文件。
core|/nucleus resetuser|nucleus.nucleus.resetuser.base|删除所有Nucleus和部分Minecraft中的玩家数据。
core|/nucleus save|nucleus.nucleus.save.base|保存所有文件。
core|/nucleus setupperms|nucleus.nucleus.setupperms.base|允许玩家向特定组添加针对USER、MOD、ADMIN规则的权限。
core|/nucleus update-messages|nucleus.nucleus.update-messages.base|找到所有语言文件中可能未包含所有占位符的文本，并用默认的翻译替换掉。

#### environment模块
模块|命令|基础权限|命令描述
-|-|-|-
environment|/lockweather|nucleus.lockweather.base|锁定特定世界的天气。
environment|/time|nucleus.time.base|获取特定世界的时间。
environment|/time set|nucleus.time.set.base|设置特定世界的时间。
environment|/weather|nucleus.weather.base|设置特定世界的天气。

#### fly模块
模块|命令|基础权限|命令描述
-|-|-|-
fly|/fly|nucleus.fly.base|设置特定玩家的飞行状态。

#### freeze-subject模块
模块|命令|基础权限|命令描述
-|-|-|-
freeze-subject|/freezeplayer|nucleus.freezeplayer.base|把一个玩家固定在某处。

#### fun模块
模块|命令|基础权限|命令描述
-|-|-|-
fun|/hat|nucleus.hat.base|把一个玩家手上的物品当成头盔佩戴在头上。
fun|/ignite|nucleus.ignite.base|让特定玩家着火。
fun|/kittycannon|nucleus.kittycannon.base|从一个人身上发射豹猫大炮。
fun|/lightning|nucleus.lightning.base|在特定玩家或特定位置处打雷。

#### geo-ip模块
模块|命令|基础权限|命令描述
-|-|-|-
geo-ip|/geoip|nucleus.geoip.base|获取玩家的保持的连接所处的国家或地区。
geo-ip|/geoip update|nucleus.geoip.update.base|更新地理位置数据库，强烈建议两次更新的时间间隔不小于一个月。

#### home模块
模块|命令|基础权限|命令描述
-|-|-|-
home|/deletehome|nucleus.home.base|删除家的位置。
home|/deletehomeother|nucleus.home.deleteother.base|删除另一个玩家的家的位置。
home|/home|nucleus.home.base|传送到家的位置。
home|/homeother|nucleus.home.other.base|传送到另一个玩家家的位置。
home|/homeset|nucleus.home.set.base|设置特定名称的家的位置。
home|/listhomes|nucleus.home.list.base|输出所有家的一个列表。

#### ignore模块
模块|命令|基础权限|命令描述
-|-|-|-
ignore|/ignore|nucleus.ignore.base|切换是否忽视所有聊天信息的状态。

#### info模块
模块|命令|基础权限|命令描述
-|-|-|-
info|/info|nucleus.info.base|获取服务器的信息。
info|/motd|nucleus.motd.base|允许查看位于motd.txt中的MOTD的信息。

#### inventory模块
模块|命令|基础权限|命令描述
-|-|-|-
inventory|/clear|nucleus.clear.base|把一个玩家的背包清空。
inventory|/enderchest|nucleus.enderchest.base|允许查看其他人的末影箱。
inventory|/invsee|nucleus.invsee.base|允许查看其他人的背包。

#### item模块
模块|命令|基础权限|命令描述
-|-|-|-
item|/enchant|nucleus.enchant.base|允许附魔物品。
item|/itemname|nucleus.itemname.base|和物品名称有关的相关操作。
item|/itemname clear|nucleus.itemname.clear.base|清除玩家手中的物品的自定义物品名称。
item|/itemname set|nucleus.itemname.set.base|设置玩家手中的物品的玩家可见名称。
item|/lore|nucleus.lore.base|和Lore有关的相关操作。
item|/lore add|nucleus.lore.set.base|为手中的物品的Lore添加一行新的。
item|/lore clear|nucleus.lore.set.base|清除手中物品的Lore的所有行。
item|/lore delete|nucleus.lore.set.base|删除手中物品的Lore的特定一行。
item|/lore edit|nucleus.lore.set.base|修改手中物品的Lore的特定一行。
item|/lore insert|nucleus.lore.set.base|在手中物品的Lore的特定一行处插入一行新的。
item|/lore set|nucleus.lore.set.base|设置手中物品的Lore，原有的Lore将被替换。
item|/more|nucleus.more.base|把手中的物品的数量加到最大。
item|/repair|nucleus.repair.base|修复手中的物品。
item|/showitemattributes|nucleus.showitemattributes.base|显示或隐藏物品提示框中的属性。
item|/skull|nucleus.skull.base|在你的背包中添加特定玩家的头颅（如果未指定玩家，添加你自己的）。

#### jail模块
模块|命令|基础权限|命令描述
-|-|-|-
jail|/checkjail|nucleus.jail.checkjail.base|检查一个玩家是否已入狱。
jail|/checkjailed|nucleus.checkjailed.base|检查狱中玩家的缓存信息，可指定特定监狱。
jail|/jail|nucleus.jail.base|设置玩家是否被关起来。
jail|/jails|nucleus.jail.list.base|显示所有的监狱。
jail|/jails delete|nucleus.jail.delete.base|删除一个监狱。
jail|/jails set|nucleus.jail.set.base|创建一个监狱。
jail|/jails tp|nucleus.jail.list.base|传送到一个监狱。

#### jump模块
模块|命令|基础权限|命令描述
-|-|-|-
jump|/jump|nucleus.jump.base|小跳到玩家所注视的方块上。
jump|/thru|nucleus.thru.base|穿墙传送。
jump|/top|nucleus.top.base|把一个特定玩家传送到地面上。
jump|/unstuck|nucleus.unstuck.base|把一个玩家推动一方块的距离以解救玩家，如果可以做到的话。

#### kick模块
模块|命令|基础权限|命令描述
-|-|-|-
kick|/kick|nucleus.kick.base|踢出一个玩家。
kick|/kickall|nucleus.kickall.base|踢出所有玩家，也可以顺道把白名单模式打开。

#### kit模块
模块|命令|基础权限|命令描述
-|-|-|-
kit|/firstjoinkit|nucleus.firstjoinkit.base|把礼包变成第一次加入的玩家自动送出的礼包。
kit|/kit|nucleus.kit.base|送出礼包。
kit|/kit add|nucleus.kit.add.base|新建礼包，并把当前背包里的物品作为新建礼包的内容。
kit|/kit autoredeem|nucleus.kit.autoredeem.base|设置或取消设置登录时是否自动送出礼包。
kit|/kit command|nucleus.kit.command.base|输出所有和礼包有关的命令列表。
kit|/kit command add|nucleus.kit.command.add.base|为特定礼包添加一条命令。
kit|/kit command clear|nucleus.kit.command.remove.base|把特定礼包的所有可用命令清空。
kit|/kit command edit|nucleus.kit.command.edit.base|打开一个游戏内GUI，其中的所有礼包以书的形式呈现。
kit|/kit command remove|nucleus.kit.command.remove.base|移除特定礼包里的一条命令。
kit|/kit cost|nucleus.kit.cost.base|设置特定礼包的花费。
kit|/kit create|nucleus.kit.create.base|打开一个箱子GUI，并通常这种交互方式设置新建的礼包内容。
kit|/kit edit|nucleus.kit.edit.base|打开一个可用于编辑礼包内容的界面。
kit|/kit give|nucleus.kit.give.base|为特定玩家送出礼包。
kit|/kit hidden|nucleus.kit.hidden.base|设置特定礼包是否显示在所有可用的礼包列表里。
kit|/kit info|nucleus.kit.info.base|获取一个礼包的相关信息。
kit|/kit list|nucleus.kit.list.base|列出所有可用的礼包。
kit|/kit onetime|nucleus.kit.onetime.base|设置或取消设置一个礼包是否只能领取一次。
kit|/kit permissionbypass|nucleus.kit.permissionbypass.base|如果配置文件设置允许，设置领取礼包是否需要额外的权限。
kit|/kit remove|nucleus.kit.remove.base|删除一个礼包。
kit|/kit resetusage|nucleus.kit.resetusage.base|重置玩家领取礼包的状态，也就是说可以立刻再领取礼包。
kit|/kit set|nucleus.kit.set.base|把当前背包里的物品设置成礼包里的。
kit|/kit setcooldown|nucleus.kit.setcooldown.base|设置礼包的冷却时间。
kit|/kit setfirstjoin|nucleus.kit.setfirstjoin.base|设置玩家第一次加入服务器时会送出的礼包。
kit|/kit toggleredeemmessage|nucleus.kit.toggleredeemmessage.base|设置送出礼包时是否提醒对方。
kit|/kit view|nucleus.kit.view.base|查看礼包的内容。

#### mail模块
模块|命令|基础权限|命令描述
-|-|-|-
mail|/mail|nucleus.mail.base|查看向你发送的邮件。
mail|/mail clear|nucleus.mail.base|清空收件箱里的所有邮件。
mail|/mail other|nucleus.mail.other.base|阅读他们邮件。
mail|/mail send|nucleus.mail.send.base|向特定玩家发邮件。

#### message模块
模块|命令|基础权限|命令描述
-|-|-|-
message|/helpop|nucleus.helpop.base|向所有管理员发邮件。
message|/message|nucleus.message.base|向特定玩家发邮件，或者向服务端控制台发邮件，使用“-”代表服务端控制台。
message|/msgtoggle|nucleus.msgtoggle.base|阻止其他玩家向你发私有消息。
message|/reply|nucleus.message.base|向之前向你发私有消息的玩家回消息。
message|/socialspy|nucleus.socialspy.base|查阅所有其他玩家发送的私有消息。

#### misc模块
模块|命令|基础权限|命令描述
-|-|-|-
misc|/blockinfo|nucleus.blockinfo.base|获取你注视的方块的所有相关信息。
misc|/entityinfo|nucleus.entityinfo.base|获取你注视的实体的所有相关信息。
misc|/feed|nucleus.feed.base|填满一个玩家的饥饿值。
misc|/god|nucleus.god.base|设置一个玩家是否免疫所有伤害。
misc|/heal|nucleus.heal.base|把一个玩家的血量填满。
misc|/iteminfo|nucleus.iteminfo.base|获取你手中的物品的信息。
misc|/ping|nucleus.ping.base|获取目标玩家的延迟。
misc|/serverstat|nucleus.serverstat.base|获取服务器的运行时信息。
misc|/servertime|nucleus.servertime.base|获取服务器的当前时间。
misc|/speed|nucleus.speed.base|设置玩家的行走或飞行速度，1代表默认速度。
misc|/suicide|nucleus.suicide.base|自杀，并显示死亡画面。

#### mob模块
模块|命令|基础权限|命令描述
-|-|-|-
mob|/spawnmob|nucleus.spawnmob.base|在特定玩家的位置生成一个怪物。

#### mute模块
模块|命令|基础权限|命令描述
-|-|-|-
mute|/checkmute|nucleus.checkmute.base|检查特定玩家是否被禁言。
mute|/checkmuted|nucleus.checkmuted.base|检查玩家的禁言信息。
mute|/globalmute|nucleus.globalmute.base|开启全员禁言，只有拥有“nucleus.globalmute.voice.auto”权限的人可以接着说话。
mute|/mute|nucleus.mute.base|堵上特定玩家的嘴。
mute|/voice|nucleus.globalmute.voice.base|在全员禁言的时候允许玩家说话。

#### nameban模块
模块|命令|基础权限|命令描述
-|-|-|-
nameban|/nameban|nucleus.nameban.base|阻止一个IGN而不是一个玩家进入服务器。
nameban|/nameunban|nucleus.nameban.unban.base|重新允许一个IGN而不是一个玩家进入服务器。

#### nickname模块
模块|命令|基础权限|命令描述
-|-|-|-
nickname|/delnick|nucleus.nick.base|移除你当前的昵称。
nickname|/nick|nucleus.nick.base|设置你的昵称（显示名称）。
nickname|/realname|nucleus.realname.base|根据玩家的昵称获取其真实id。

#### note模块
模块|命令|基础权限|命令描述
-|-|-|-
note|/checknotes|nucleus.checknotes.base|允许检查玩家的标记。
note|/clearnotes|nucleus.clearnotes.base|允许清除玩家的标记。
note|/note|nucleus.note.base|允许为玩家添加一个标记。
note|/removenote|nucleus.removenote.base|允许移除一个和特定玩家相关联和标记。

#### playerinfo模块
模块|命令|基础权限|命令描述
-|-|-|-
playerinfo|/getfromip|nucleus.getfromip.base|获取所有玩家上次上线时使用的IP。
playerinfo|/getpos|nucleus.getpos.base|获取一个命令执行者或玩家的位置。
playerinfo|/list|nucleus.list.base|列出所有在线的玩家。
playerinfo|/seen|nucleus.seen.base|获取特定玩家的相关信息。

#### powertool模块
模块|命令|基础权限|命令描述
-|-|-|-
powertool|/powertool|nucleus.powertool.base|把当前手上的物品使用特定的命令绑定。
powertool|/powertool delete|nucleus.powertool.base|移除当前手上的物品的所有命令绑定。
powertool|/powertool list|nucleus.powertool.base|列出所有当前玩家已绑定的物品。
powertool|/powertool toggle|nucleus.powertool.base|设置物品的命令绑定是否启用。

#### rtp模块
模块|命令|基础权限|命令描述
-|-|-|-
rtp|/rtp|nucleus.rtp.base|允许玩家向一个随机的方向传送，只要目标位置在世界边界内。

#### rules模块
模块|命令|基础权限|命令描述
-|-|-|-
rules|/rules|nucleus.rules.base|查看服务器的规则。

#### server-list模块
模块|命令|基础权限|命令描述
-|-|-|-
server-list|/serverlist|nucleus.serverlist.base|方便修改Server List的相关操作。
server-list|/serverlist message|nucleus.serverlist.message.base|允许在定时的基础上临时修改Server List的MOTD。

#### server-shop模块
模块|命令|基础权限|命令描述
-|-|-|-
server-shop|/itembuy|nucleus.itembuy.base|允许玩家以特定价格购买服务器设定的物品。
server-shop|/itemsell|nucleus.itemsell.base|允许玩家以服务器设定的价格卖出手上物品。
server-shop|/itemsellall|nucleus.itemsellall.base|允许玩家以服务器设定的价格卖出背包里所有同类型的物品。
server-shop|/setworth|nucleus.setworth.base|允许玩家设置物品买卖的价格。
server-shop|/worth|nucleus.worth.base|允许玩家查看服务器中等待买卖的物品的价格。

#### spawn模块
模块|命令|基础权限|命令描述
-|-|-|-
spawn|/firstspawn|nucleus.firstspawn.base|如果已设置，传送到新玩家的出生点。
spawn|/setfirstspawn|nucleus.firstspawn.set.base|设置新玩家的出生点。
spawn|/setfirstspawn del|nucleus.firstspawn.remove.base|移除新玩家的出生点。
spawn|/setspawn|nucleus.setspawn.base|设置当前世界的出生点。
spawn|/spawn|nucleus.spawn.base|传送到当前世界的出生点。
spawn|/spawn other|nucleus.spawn.other.base|把其他玩家传送到当前世界的出生点。

#### staff-chat模块
模块|命令|基础权限|命令描述
-|-|-|-
staff-chat|/staffchat|nucleus.staffchat.base|允许在管理员聊天频道聊天。

#### teleport模块
模块|命令|基础权限|命令描述
-|-|-|-
teleport|/teleport|nucleus.teleport.teleport.base|把一个玩家传送到另一个位置。
teleport|/teleportnative|无|/minecraft:tp的别名。
teleport|/tpa|nucleus.teleport.tpa.base|向其他玩家发送一个传送到对方那里的请求。
teleport|/tpaall|nucleus.teleport.tpaall.base|向其他所有玩家发送一个传送到自己所在位置的请求。
teleport|/tpaccept|nucleus.teleport.tpaccept.base|允许传送请求。
teleport|/tpahere|nucleus.teleport.tpahere.base|向其他玩家发送一个传送到自己所在位置的请求。
teleport|/tpall|nucleus.teleport.tpall.base|使其他所有玩家强制传送到自己所在位置。
teleport|/tpdeny|nucleus.teleport.tpdeny.base|不允许传送请求。
teleport|/tphere|nucleus.teleport.tphere.base|把一个玩家传送到自己所在位置。
teleport|/tppos|nucleus.teleport.tppos.base|传送到特定位置。
teleport|/tptoggle|nucleus.teleport.tptoggle.base|设置是否收到传送请求。

#### vanish模块
模块|命令|基础权限|命令描述
-|-|-|-
vanish|/vanish|nucleus.vanish.base|设置在服务器中是否可见。

#### warn模块
模块|命令|基础权限|命令描述
-|-|-|-
warn|/checkwarnings|nucleus.checkwarnings.base|允许检查特定玩家的警告。
warn|/clearwarnings|nucleus.clearwarnings.base|允许清除特定玩家的警告。
warn|/removewarning|nucleus.removewarning.base|允许移除一条针对特定玩家的警告。
warn|/warn|nucleus.warn.base|允许为玩家添加警告。

#### warp模块
模块|命令|基础权限|命令描述
-|-|-|-
warp|/warp|nucleus.warp.base|传送到特定的传送点。
warp|/warp category|nucleus.warp.category.base|用于管理传送点种类的相关指令。
warp|/warp category list|nucleus.warp.category.list.base|列出所有服务器上正在使用的传送点种类。
warp|/warp category removedescription|nucleus.warp.category.description.base|移除一种种类的描述。
warp|/warp category removedisplayname|nucleus.warp.category.displayname.base|移除一种种类的名称。
warp|/warp category setdescription|nucleus.warp.category.description.base|设置一种种类的描述。
warp|/warp category setdisplayname|nucleus.warp.category.displayname.base|设置一种种类的名称。
warp|/warp cost|nucleus.warp.cost.base|设置传送到特定传送点的花费。
warp|/warp delete|nucleus.warp.delete.base|移除特定传送点。
warp|/warp list|nucleus.warp.list.base|列出所有服务器可用的传送点。
warp|/warp set|nucleus.warp.set.base|设置特定传送点。
warp|/warp setcategory|nucleus.warp.setcategory.base|设置在/warp list命令中出现的传送点种类。
warp|/warp setdescription|nucleus.warp.setdescription.base|设置或使用“-r”移除传送点的描述。


#### world模块
模块|命令|基础权限|命令描述
-|-|-|-
world|/world|nucleus.world.base|所有和世界相关的命令的父命令。
world|/world border|nucleus.world.border.base|所有和世界边界相关的命令的父命令。
world|/world border cancelgen|nucleus.world.border.gen.base|取消所有世界边界设置。
world|/world border gen|nucleus.world.border.gen.base|生成直到世界边界的所有区块。
world|/world border reset|nucleus.world.border.set.base|重置世界边界至默认值。
world|/world border set|nucleus.world.border.set.base|设置世界边界。
world|/world create|nucleus.world.create.base|创建一个拥有特定名称和选项的世界。
world|/world delete|nucleus.world.delete.base|从存储介质中把世界移除。
world|/world disable|nucleus.world.disable.base|关闭一个未加载的世界，使其不再被加载。
world|/world enable|nucleus.world.enable.base|启用一个世界，使其在需要的时候被加载。
world|/world gamerule|nucleus.world.gamerule.base|查看一个世界的所有规则。
world|/world gamerule set|nucleus.world.gamerule.set.base|设置特定世界的规则。
world|/world generators|nucleus.world.create.base|查看创建世界所使用的生成器规则。
world|/world info|nucleus.world.list.base|查看一个世界的所有相关信息。
world|/world list|nucleus.world.list.base|列出所有可用的世界。
world|/world load|nucleus.world.load.base|列出所有未被加载的世界。
world|/world modifiers|nucleus.world.create.base|查看创建世界所使用的参数。
world|/world presets|nucleus.world.create.base|查看创建世界所使用的预设。
world|/world setdifficulty|nucleus.world.setdifficulty.base|设置一个世界的游戏难度。
world|/world setgamemode|nucleus.world.setgamemode.base|设置一个世界的游戏模式。
world|/world sethardcore|nucleus.world.sethardcore.base|设置一个世界是否启用极限模式。
world|/world setkeepspawnloaded|nucleus.world.setkeepspawnloaded.base|设置一个世界是否持续加载出生点区块。
world|/world setloadonstartup|nucleus.world.setloadonstartup.base|设置一个世界在服务端启动的时候是否加载。
world|/world setpvpenabled|nucleus.world.setpvpenabled.base|设置一个世界是否允许PVP。
world|/world setspawn|nucleus.world.setspawn.base|设置一个世界的出生点。
world|/world spawn|nucleus.world.spawn.base|传送到一个世界的出生点。
world|/world teleport|nucleus.world.teleport.base|传送自己或者其他玩家到一个特定的世界。
world|/world unload|nucleus.world.unload.base|使一个已经加载的世界变成未加载状态。
