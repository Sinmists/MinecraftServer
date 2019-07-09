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
