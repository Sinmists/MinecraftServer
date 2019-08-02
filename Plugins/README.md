# Minecraft Server Plugins

**这是什么文档？**

目前计划长期运行的Minecraft服务器建设中，插件配置需要频繁翻阅不同文档，且每个插件常用的只有部分文档内容而已，因此按照个人需求整合并修改了部分配置内容，可参考但不建议全盘搬运，因此造成的后果概不负责，请以官方文档为准。

正弦默示录  
2019.7.9

---

**目录**
[TOC]

---

## [主模组]Sponge

## 安装

在原版server.jar文件上安装forge，下载[合适版本](https://www.spongepowered.org/downloads/)Sponge放入Mod文件夹即可，注意forge与sponge版本号需相同。

### 权限配置

```
给与服主 `sponge` 权限

给与默认玩家权限组以下权限：

/help
sponge.command.help 让玩家查看可用命令列表
```

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

建议创建权限组后将管理员添加到权限组，而非直接给与管理员权限

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

```
1.设置前缀
让owner权限组的玩家拥有 "&4[腐竹] " 前缀，在admin权限组的玩家拥有 "&c[管理员] " 前缀的话，运行：
/lp creategroup owner
/lp creategroup admin
/lp group owner parent add admin
/lp group owner meta addprefix 100 "&4[腐竹] "
/lp group admin meta addprefix 90 "&c[管理员] "
如果要将owner用户组的称号改为使用 "&5" 这个颜色代码的话，要想删除之前设定的值，需要运行：
/lp group owner meta removeprefix 100
这会将所有设定给admin权限组的，优先级为100的前缀全部移除
```
```
2.设置默认玩家Home上限为2个
/lp group default meta set home-count 2
注意继承关系，高级权限组按需设置更大数值
```

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

##### 元数据 (/lp user \ meta ... | /lp group \ meta ...)
```
/lp user/group <user|group> meta info
权限: luckperms.user.meta.info or luckperms.group.meta.info
显示用户/组的继承元数据，前缀和后缀。

/lp user/group <user|group> meta set
权限: luckperms.user.meta.set or luckperms.group.meta.set
参数:

<key> - 设置的键值
<value> - 设置的键值的值
[context...] - 设置的元数据的上下文
设置用户/组的键值对元数据，这些值可以用于读取并且可以通过其他使用 Vault 或者 Sponge Permissions API 的插件更改。

/lp user/group <user|group> meta unset
权限: luckperms.user.meta.unset or luckperms.group.meta.unset
参数:

<key> - 取消设置的键
[context...] - 取消设置的元数据的上下文
取消设置一个用户或组的元数据键值。

/lp user/group <user|group> meta settemp
权限: luckperms.user.meta.settemp or luckperms.group.meta.settemp
参数:

<key> - 设置的键值
<value> - 设置的键的值
<duration> - 元数据过期的时间
[context...] - 设置的元数据的上下文
给一个玩家/组设置临时元数据键值，提供 false 值将会否定这个权限。持续时间应为时间段或者一个标准的 Unix 时间戳，比如 "3d13h45m" 将会设置权限在 3 天, 13 小时 45 分钟后过期。"1482694200" 会设置过期时间为 7:30PM 于 25th December 2016。

/lp user/group <user|group> meta unsettemp
权限: luckperms.user.meta.unsettemp or luckperms.group.meta.unsettemp
参数:

<key> - 取消设置的键
[context...] - 取消设置的元数据的上下文
取消设置一个用户或组的临时元数据。

/lp user/group <user|group> meta addprefix
权限: luckperms.user.meta.addprefix or luckperms.group.meta.addprefix
参数:

<priority> - 添加前缀的优先度
<prefix> - 实际的前缀字符串
[context...] - 添加前缀的上下文
给一个玩家/组设置前缀，使用 " " 来添加空格。

/lp user/group <user|group> meta addsuffix
权限: luckperms.user.meta.addsuffix or luckperms.group.meta.addsuffix
参数:

<priority> - 添加后缀的优先度
<suffix> - 实际的后缀字符串
[context...] - 添加后缀的上下文
给一个玩家/组设置后缀，使用 " " 来添加空格。

/lp user/group <user|group> meta removeprefix
权限: luckperms.user.meta.removeprefix or luckperms.group.meta.removeprefix
参数:

<priority> - 移除前缀的优先度
<prefix> - 实际的前缀字符串
[context...] - 添加前缀的上下文
给一个玩家/组移除前缀，使用 " " 来添加空格。

/lp user/group <user|group> meta removesuffix
权限: luckperms.user.meta.removesuffix or luckperms.group.meta.removesuffix
参数:

<priority> - 移除后缀的优先度
<suffix> - 实际的后缀字符串
[context...] - 添加后缀的上下文
给一个玩家/组移除后缀，使用 " " 来添加空格。

/lp user/group <user|group> meta addtempprefix
权限: luckperms.user.meta.addtempprefix or luckperms.group.meta.addtempprefix
参数:

<priority> - 添加前缀的优先度
<prefix> - 实际的前缀字符串
<duration> - 前缀过期的时间
[context...] - 添加前缀的上下文
给一个玩家/组设置临时前缀，提供 false 值将会否定这个权限。持续时间应为时间段或者一个标准的 Unix 时间戳，比如 "3d13h45m" 将会设置权限在 3 天, 13 小时 45 分钟后过期。"1482694200" 会设置过期时间为 7:30PM 于 25th December 2016。

/lp user/group <user|group> meta addtempsuffix
权限: luckperms.user.meta.addtempsuffix or luckperms.group.meta.addtempsuffix
参数:

<priority> - 添加后缀的优先度
<suffix> - 实际的后缀字符串
<duration> - 后缀过期的时间
[context...] - 添加后缀的上下文
给一个玩家/组设置临时后缀，提供 false 值将会否定这个权限。持续时间应为时间段或者一个标准的 Unix 时间戳，比如 "3d13h45m" 将会设置权限在 3 天, 13 小时 45 分钟后过期。"1482694200" 会设置过期时间为 7:30PM 于 25th December 2016。

/lp user/group <user|group> meta removetempprefix
权限: luckperms.user.meta.removetempprefix or luckperms.group.meta.removetempprefix
参数:

<priority> - 移除前缀的优先度
<prefix> - 实际的前缀字符串
[context...] - 添加前缀的上下文
给一个玩家/组移除临时前缀，使用 " " 来添加空格。

/lp user/group <user|group> meta removetempsuffix
权限: luckperms.user.meta.removetempsuffix or luckperms.group.meta.removetempsuffix
参数:

<priority> - 移除后缀的优先度
<suffix> - 实际的后缀字符串
[context...] - 添加后缀的上下文
给一个玩家/组移除临时后前缀，使用 " " 来添加空格。

/lp user/group <user|group> meta clear
权限: luckperms.user.meta.clear or luckperms.group.meta.clear
参数:

[context...] - 用于过滤的上下文
移除所有的元数据，前后缀。
```
---

## [基础指令]Nucleus

[官网](https://nucleuspowered.org/index.html) | 
[中文文档](http://www.mcbbs.net/thread-787354-1-1.html)

### 安装

下载[最新版本](https://ore.spongepowered.org/Nucleus/Nucleus/versions)放入Mod文件夹即可

#### 初始化配置文件

##### \config\nucleus\main.conf

- AFK
`messages {` 修改afk状态玩家接收到的信息

- Core
`kick-on-stop {` 服务器关闭前踢出所有玩家设置为true后，可修改呈现给玩家的信息
`offline-user-tab-limit=` 确定离线玩家的最大数量

- Home
`respawn-at-home=` 玩家重生将会回到他们默认名为home的家

- Info
`motd-title=` 用于展示登录信息MOTD的标题

- Name Banning
`default-reason=` 修改用户名不合法的提示信息

- Player Info
`enabled=` 启用，`/list`指令将会按用户组显示玩家列表

- Protection
`disable-crop-trample` 禁止踩踏农田
`mob-griefing` 禁止生物破坏地形

- Rules
`rules-title=` 规则界面的标题

##### \config\nucleus\info

可在该文件夹下添加`.txt`文件，在使用`/info`命令时查看各文件内容

**注意！编码需改为UTF-8，否则会显示乱码**

### 权限分配

默认将权限分为以下5类：
- `USER` - 普通玩家权限
- `MOD` - 对玩家行为进行审核的权限，包括ban、jail等
- `ADMIN` - 改变游戏机制与服务器管理方式的权限，包括绝大多数权限
- `OWNER` - 少数可能导致严重服务器损坏的权限，通常只应由具有最终权限的人员使用
- `NONE` - 高度专业化的权限，例如隐身登录，应根据具体情况决定，包括/nucleus setupperms初始化权限命令

在[这里](https://nucleuspowered.org/docs/permissions.html)查看权限所属权限类别

使用如下命令将权限按类别赋予权限组：
`/nucleus setupperms <ROLE> <group>`
例如将`USER`权限类赋予默认玩家权限组`default`：
`/nucleus setupperms USER default`

**默认玩家权限中没有`back`指令的使用权，请手动给予`default`权限组以`nucleus.back.base`权限**

**注意！** 高权限类并不继承低权限类的权限，建议使用权限管理插件的`parent`命令继承低权限组

**警告！！** 不要再任何时候给玩家"*"权限，这种用法为Bukkit时代的习惯用法，但是并不适用于Sponge！如果给与，那么他将会永久在登录服务器的时候保持隐身状态！

### 常用命令解析

`/nucleus reload` 重新加载插件配置，但如果涉及功能的启用或禁用需要重启服务器
`/serverstat` 显示服务器状态
`/stop [reason]` 覆盖默认服务器关闭/重启的时候，呈现给玩家的信息
`/speed` 获得最大速度增益

### 所有命令

版本更新会添加更多命令，在[这里](https://nucleuspowered.org/docs/commands2.html)查看最新所有命令

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

---

## [经济]EconomyLite

[官方文档](http://flibiostudio.github.io/EconomyLiteDocs/) | 
[中文文档](http://www.mcbbs.net/thread-726623-1-1.html)

### 安装

下载[最新版本](https://ore.spongepowered.org/Flibio/EconomyLite/versions)放入Mod文件夹即可

#### 初始化配置文件

##### \config\economylite\messages.conf
复制以下内容并覆盖
```
command-balance="&6当前余额: &a{balance} &6{label}"
command-balanceother="&6{player}的余额: &a{balance} &6{label}"
command-baltop-data="&7[&f{position}&7] &e{name}: &a{balance} &e{label}"
command-baltop-head="&6&l土豪榜"
command-baltop-invalidpage="&c没有这个页码!"
command-baltop-navigation="&7[&a{button}&7]"
command-baltop-nodata="&c没找到任何数据!"
command-currency-changed="&6货币已改成&a{currency}!"
command-currency-confirm="&6点击该消息以确认切换到&a{currency}!"
command-currency-created="&6已注册新货币: &a{currency}!"
command-currency-current="&6当前的货币: &a{currency}!"
command-currency-delete="&6执行 &a/currency delete <currency> &6或点击货币来删除一种货币!"
command-currency-deleteconfirm="&6点击该消息以确认删除&a{currency}!"
command-currency-deleted="&6成功删除货币&a{currency}!"
command-currency-deletedefault="&c你不能删除当前的默认货币!"
command-currency-exists="&c该货币已存在!"
command-currency-invalid="&c未找到这种货币!"
command-currency-selectnew="&6执行 &a/currency set <currency> &6或点击新货币来改变货币!"
command-econ-addfail="&c给{name}余额添加货币失败!"
command-econ-addsuccess="&a成功给&6{name}&a余额添加货币!"
command-econ-notify="&6你的余额被修改了!"
command-econ-removefail="&c无法删除{name}余额中的货币!"
command-econ-removesuccess="&a成功删除&6{name}&a余额中的货币!"
command-econ-setfail="&c无法给{name}设置余额!"
command-econ-setsuccess="&a成功给&6{name}设置余额!"
command-error="&c出现内部错误!"
command-invalidsource="&c你必须是一个{sourcetype}来使用此指令!"
command-migrate-completed="&a转换完毕!"
command-migrate-confirm="&c转换系统将会覆盖现有的数据- &6请执行 &a/migrate <mode> yes &6来确认转换!"
command-migrate-fail="&c转换失败!"
command-migrate-nomode="&c请选择有效的转换模式!"
command-noperm="&c你没权限使用该指令!"
command-pay-confirm="&a{player}不在线! 你仍然想寄钱过去吗?"
command-pay-confirmno="&c{player}没确认支付!"
command-pay-failed="&c为 {target} 付款失败!"
command-pay-invalid="&c指定的货币无效!"
command-pay-notyou="&c你不能给你自己打钱!"
command-pay-success="&6{target} &a成功支付 &6{amountandlabel}&a!"
command-pay-target="&6你收到了来自&a{sender}&6的&a{amountandlabel}!"
command-refresh-fail="&c重置数据库缓存失败!"
command-refresh-success="&a重置数据库缓存成功!"
command-usage="&a用法: &f{command} {subcommands}"
module-loan-ask="&6你想获取贷款 &a{amount} &6{label}? (输入 &a/loan accept &6或 &a/loan deny&6)"
module-loan-balance="&6当前贷款余额: &a{balance} &6{label}"
module-loan-fail="&c贷款时发生错误!"
module-loan-full="&c你的贷款余额已满，请先偿还一些贷款!"
module-loan-interest="&6贷款的利率为 &a{rate}&6!"
module-loan-no="&c贷款已被取消!"
module-loan-noloan="&c你还没有提供贷款!"
module-loan-partial="&c你获得了一些贷款!"
module-loan-payed="&6支付 &a{amount} &6{label} 到了你的贷款余额!"
module-loan-payedfail="&c贷款支付失败!"
module-loan-payment="&6将添加 &a{amount} &6{label} 到你的贷款余额!"
module-loan-yes="&a贷款成功!"
```

##### \config\economylite\currencies.conf

`plural` 复数货币
`singular` 单数货币
`symbol` 符号，可用¥
`current` 当前使用的货币


### 命令与权限

玩家指令：
```
/balance [<player>] - 查看余额
权限: economylite.balance : economylite.admin.balanceothers
/pay <player> <amount> - 给其他玩家打钱.
权限: economylite.pay
/payv <account> <amount> - 给虚拟账户打钱
权限: economylite.virtual.pay
/baltop [<page>] - 查看金钱排行榜.
权限: economylite.baltop
```

贷款系统指令：
```
/loan - 查看基本的贷款命令
权限: economylite.loan
/loan balance - 查看你贷款的余额
权限: economylite.loan.balance
/loan take <amount> - 贷款指定金额
权限: economylite.loan.take
/loan pay <amount> - 为你的贷款余额付钱.
权限: economylite.loan.pay
/loan accept - 接受一个贷款
权限: economylite.loan.accept
/loan deny - 否认一个贷款
权限: economylite.loan.deny
```

管理员指令：
```
/econ set | add | remove <player> <amount> - 添加或设置或删除指定玩家的余额.
权限: economylite.admin.econ : economylite.admin.econ.set : economylite.admin.econ.add : economylite.admin.econ.remove
/econ setall <amount> - 设置服务器上所有人的余额
权限: economylite.admin.econ.setall (Also requires economylite.admin.econ)
/currency set | delete <currency> - 设置或删除或显示货币
权限: economylite.admin.currency : economylite.admin.currency.set : economylite.admin.currency.delete
/currency create <singular> <plural> <symbol> - 创建一种新的货币
权限: economylite.admin.currency.create
/vbalance <account> - 查看虚拟账户的余额
权限: economylite.admin.virtual.balance
/vecon set | add | remove <account> <amount> - 添加或设置或删除虚拟账户的余额
权限: economylite.admin.virtual.set : economylite.admin.virtual.add : economylite.admin.virtual.remove
/vpay <player> <amount> - 给指定玩家的虚拟账户打钱
权限: economylite.admin.virtual.pay
/migrate <mode> [<confirm>] - 将其他经济插件的数据迁移到EconomyLite
```

---

## [领地]GriefPrevention

[官方文档](https://github.com/MinecraftPortCentral/GriefPrevention/wiki/Getting-Started) | 
[中文文档](https://pluginscdtribe.github.io/wiki/griefprevention/)

### 安装

下载[最新版本](https://ore.spongepowered.org/blood/GriefPrevention/versions)放入Mod文件夹即可

### 配置文件初始化

#### \config\griefprevention\worlds\global.conf

- claim

`auto-chest-claim-block-radius=-1` 设置放下箱子自动设置领地的半径

- general

`limit-pistons-to-claims=true` 限制从领地外用活塞推领地内方块

`limit-tree-growth=true` 限制领地外的树生长到领地内

`max-claim-inspection-distance=100` 持木棍右键检查附近领地的范围半径

- logging

`suspicious-activity=true` 记录可疑日志

- playerdata

`claim-block-system=AREA` 领地方块计数默认2D模式

### 选项初始化

`lp group default meta set griefprevention.blocks-accrued-per-hour 30` 每小时在线赚取的领地块数量,10小时累积到20长15宽

`lp group default meta set griefprevention.claim-create-mode 0` 玩家默认登录圈地方式为2D模式

`lp group default meta set griefprevention.create-claim-limit-basic 1` 每个玩家最大基础领地为1个

`lp group default meta set griefprevention.initial-claim-blocks 0` 玩家初始领地方块数量为0块

`lp group default meta set griefprevention.max-accrued-claim-blocks 300` 通过在线时长累积的最多领地方块数量为200块

`lp group default meta set griefprevention.claim-expiration-basic 90` 超过90天不上线的玩家基本领地将被删除

设置完成后使用 `gpreload` 激活配置

### 权限配置

给与管理员权限组 `griefprevention.admin` ，默认玩家权限组 `griefprevention.user`

**注意！不要给与 `griefprevention.*` 权限以免插件报错**

#### 领地
权限|描述
-|-
griefprevention.user.claim.command.abandon|允许放弃领地
griefprevention.user.claim.command.abandon-all|允许放弃所有领地
griefprevention.user.claim.command.abandon-top-level|允许放弃一个领地以及所有子区域
griefprevention.user.claim.command.cuboid|允许切换长方体选择模式
griefprevention.user.claim.command.list|允许列出玩家领地列表
griefprevention.user.claim.command.basic-mode|允许使用领地铲
griefprevention.user.claim.command.give.book|允许获得一本领地指南
griefprevention.user.claim.command.give.pet|允许玩家送出一直已经驯服的宠物
griefprevention.user.claim.command.info.others|允许获得其它领地的信息
griefprevention.user.claim.command.info.base|允许获得领地信息
griefprevention.user.claim.command.info.teleport.others|允许用户在其它领地的信息页使用传送
griefprevention.user.claim.command.info.teleport.base|允许用户在领地信息页使用传送
griefprevention.user.claim.command.name|允许设置领地名称
griefprevention.user.claim.command.farewell|允许设置领地告别语
griefprevention.user.claim.command.greeting|允许设置领地欢迎语
griefprevention.user.claim.command.set-spawn|允许设置领地出生点
griefprevention.user.claim.command.spawn|允许使用领地出生点
griefprevention.user.claim.command.subdivide-mode|允许使用子区域铲
griefprevention.user.claim.command.transfer|允许转移自己的领地
griefprevention.user.claim.command.buy-blocks|允许购买领地 (需经济插件支持)
griefprevention.user.claim.command.sell-blocks|允许出售领地 (需经济插件支持)
griefprevention.user.claim.command.list-flags|允许列出领地属性列表
griefprevention.user.claim.command.ban-item|允许在领地内封禁物品
griefprevention.user.claim.command.unban-item|允许在领地内解禁物品
griefprevention.user.claim.command.siege|允许使用围攻指令
griefprevention.user.claim.command.inherit|允许切换从父领地继承
griefprevention.user.claim.create|允许创建领地
griefprevention.user.claim.cuboid.basic|允许创建/重新设置3D领地
griefprevention.user.claim.cuboid.subdivision|允许创建/重新设置3D子区域
griefprevention.user.claim.resize|允许重新设置领地大小
griefprevention.user.claim.siege.immune|不会被包围
griefprevention.user.claim.visualize|使自己的领地可视化
griefprevention.user.claim.visualize.nearby|使周围的领地可视化
griefprevention.user.command.info.base|获得关于自己的信息
griefprevention.user.command.info.others|获得关于其它玩家的信息

#### 属性
权限|描述
-|-
griefprevention.user.claim.flag|允许玩家在自己的领地上编辑“使用领地属性”
griefprevention.user.claim.command.flag.base|允许使用关于领地属性的指令
griefprevention.user.claim.command.flag.debug|允许开启领地属性debug模式
griefprevention.user.claim.command.flag.player|允许使用关于领地属性的玩家指令
griefprevention.user.claim.command.flag.group|允许使用关于领地属性的组指令
griefprevention.user.claim.command.flag.reset|允许使用领地属性重置指令

#### 管理
权限|描述
-|-
griefprevention.admin.claim.command.adjust-claim-blocks|允许编辑扩大领地范围
griefprevention.admin.claim.command.admin-mode|允许使用管理员领地铲模式
griefprevention.admin.claim.command.clear|
griefprevention.admin.claim.command.permission-group|允许使用关于组权限的领地指令
griefprevention.admin.claim.command.permission-player|允许使用关于玩家权限的领地指令
griefprevention.admin.claim.command.debug|
griefprevention.admin.claim.command.delete.base|允许使用删除领地指令
griefprevention.admin.claim.command.delete.basic|允许删除普通领地
griefprevention.admin.claim.command.delete.admin|允许删除管理员领地
griefprevention.admin.claim.command.delete-claims|允许删除另一个玩家的所有领地
griefprevention.admin.command.delete-admin-claims|允许删除所有管理员领地
griefprevention.admin.claim.command.list.admin|允许列出管理员领地
griefprevention.admin.command.set-accrued-claim-blocks|允许编辑领地方块数量
griefprevention.admin.command.restore-nature.base|
griefprevention.admin.command.restore-nature.aggressive|
griefprevention.admin.command.restore-nature.fill|
griefprevention.admin.command.reload|允许重载插件
griefprevention.admin.claim.set-admin-flags|允许在管理员领地里编辑领地属性
griefprevention.admin.claim.list.basic|允许列出所有普通领地
griefprevention.admin.claim.flag|允许编辑所有属性
griefprevention.admin.flag-defaults|允许编辑默认属性
griefprevention.admin.flag-overrides|允许编辑忽视属性
griefprevention.admin.claim.wilderness|允许编辑野外领地
griefprevention.admin.claim.manage.options|允许为玩家和组设置属性
griefprevention.admin.claim.command.ignore.base|允许使用无视领地的指令
griefprevention.admin.claim.command.ignore.basic|允许无视普通领地的属性
griefprevention.admin.claim.command.ignore.admin|允许无视管理员领地的属性
griefprevention.admin.claim.command.ignore.wilderness|允许无视野外领地的属性
griefprevention.admin.claim.cuboid|
griefprevention.admin.claim.resize.admin|允许重新划定管理员领地的大小
griefprevention.admin.claim.resize.admin.subdivision|允许重新划定管理员子领地的大小
griefprevention.admin.claim.resize.basic|允许重新划定普通领地的大小
griefprevention.admin.claim.resize.basic.subdivision|允许重新划定普通子领地的大小
griefprevention.admin.claim.override.resize|
griefprevention.admin.claim.override.limit|允许忽视领地大小限制
griefprevention.admin.eavesdrop.signs|
griefprevention.admin.command.unlock-drops|
griefprevention.admin.no-pvp-immunity

#### 聊天
权限|描述
-|-
griefprevention.user.chat.command.ignore|允许无视玩家
griefprevention.user.chat.command.unignore|允许解除无视玩家
griefprevention.admin.chat.command.list|允许列出无视玩家列表
griefprevention.admin.chat.command.separate|允许分开玩家
griefprevention.admin.chat.command.unseparate|允许解除玩家分开
griefprevention.admin.chat.command.softmute|允许禁言玩家
griefprevention.admin.chat.not-ignorable|有此权限的玩家不能被无视
griefprevention.admin.chat.spam-override|有此权限的玩家不会被语言过滤影响
griefprevention.admin.chat.eavesdrop|允许玩家看到禁言内容

#### 帮助
权限|描述
-|-
griefprevention.user.command.help|允许查看帮助

#### 信任
权限|描述
-|-
griefprevention.user.claim.command.trust.access|允许添加客人
griefprevention.user.claim.command.trust.container|允许添加保管员
griefprevention.user.claim.command.trust.build|允许添加建造者
griefprevention.user.claim.command.trust.permission|允许添加管理员
griefprevention.user.claim.command.trust.list|允许列出信任玩家列表
griefprevention.user.claim.command.trust.remove|允许取消信任玩家

### 命令

#### 领地

```
/abandonclaim
删除你所在的领地。

/abandonallclaims
删除你所有的领地。

/abandontopclaim
删除你所在的领地以及其所有子领地。

/basicclaims
别名: bc

切换圈地模式为基础圈地模式。

/claim
将WorldEditor所选区域声明为一个领地. 贴士: 如果服务器没有安装WE插件，这个指令将会失效。

/claimbook [player]
给予指定玩家一本认领指南。（译者注：YouTube外链，慎用）

/claimfarewell <"message">
设置你所在领地的道别语（当玩家离开领地时的提示语）。

删除道别语请使用 ：/claimfarewell ""

/claimgreeting <"message">
设置你所在领地的问候语（当玩家进入领地时的提示语）。

删除问候语请使用 ： /claimgreeting ""

/claimbuy
查看正在出售的领地列表。单击 [Buy] 进行购买。

/claimsell [price]
以指定的价格出售你所在的领地，取消出售时，请将price设为 -1 或在 /claiminfo中的 ForSale 设置为为 false。

/buyclaimblocks [numberOfBlocks]
别名: buyclaim

使用服务器货币购买额外的领地上限。若服务器没有经济插件则此指令失效（译者注：如果配置文件中关闭了领地块买卖功能，此指令也失效）。

/sellclaimblocks [numberOfBlocks]
别名: sellclaim

以服务器货币卖出你的领地块上限。若服务器没有经济插件则此指令失效（译者注：如果配置文件中关闭了领地块买卖功能，此指令也失效）。

/cuboid
别名: cuboidclaims

切换三维认领模式。

/inherit
别名: inheritpermissions

切换到细分继承模式（译者注：子领地）。

/claimlist [<player> [world]]
别名: claimslist

列出指定玩家的可认领领地块上限和现有领地信息。

/claiminfo [id]
别名: claimsinfo

查看你所在的（或通过领地id查询指定的）领地信息。

/claimsetspawn
别名: claimsetspawn

将你的当前位置设置为领地的传送点。

/claimspawn
别名: claimspawn

将你传送到指定领地的可用传送点。

/claimsubdivide
别名: subdivideclaims, sc

切换到细分模式，用于细分你的领地。

/claimtransfer [player]
别名: transferclaim

将你所在位置领地的所有权转移给一个玩家。

/givepet cancel|<player>
将你驯养的宠物放生或送给他人。

/claimname ["name"]
给你的领地重命名。

/playerinfo <player> <world>|<player>|[<world>]
查看一个玩家的信息。
```

#### 选项（Flag）

```
/claimflagdebug
别名: cfd

切换到领地选项（Flag）的调试模式。

/claimflag [<flag> <source> <target> <value> [context] | <target> <value> [context]]
别名: cf

查看/设置你所在领地的选项（Flag）。

/claimflaggroup <group> <flag> <source> <target> <value> | <target> <value>
别名: cfg

查看/设置你所在领地关于制定权限组的选项（Flag）。

/claimflagplayer <player> <flag> <source> <target> <value> | <target> <value>
别名: cfp

为指定玩家设定管理选项（Flag）。

/claimflagreset
别名: cfr

将领地的管理选项（Flag）还原为默认值。
```

#### 选项

```
/claimoption [<option> <value>]
别名: co

查看/设置当前领地中的选项。

/claimoptiongroup <group> [<option> <value>]
别名: cog

查看/设置指定权限组在当前领地中的选项。

/claimoptionplayer <player> [<option> <value>]
别名: cop

查看/设置指定玩家在当前领地中的选项。
```

#### 信任

```
/accesstrust <player> <group>
别名: at
授予玩家进入你的领地和与床进行交互的权限。

/containertrust <player>|<group>
别名: ct
授予玩家进入你的领地和与容器、庄稼、动物、床、按钮和杠杆进行交互的权限。

/trust <player>|<group>
别名: t
授予玩家当前领地的最高使用权限。

/trustall <player>|<group>
别名: ta
授予玩家你所有领地的最高使用权限。

/permissiontrust <player>|<group>
别名: pt
授予玩家授予他人权限的权限。

/untrust <player>|<group>
别名: ut
将指定玩家从你领地的trust列表中移除。

/untrustall <player>|<group>
别名: uta
将指定玩家从你所有领地的trust列表中移除。

/trustlist
列出当前领地的trust授权玩家。
```

#### 管理员

```
/adjustbonusclaimblocks <player> <amount> [world]
别名: acb

增加/减去指定玩家的现有领地块。

/setaccruedclaimblocks <player> <amount> [<world>]
别名: scb

设置指定玩家的领地块认领上限（区分bonus与accrued领地块区别，可用playerinfo查看）。

/adminclaims
别名: ac

切换至管理员圈地模式。

/adminclaimlist [world]
别名: adminclaimslist, claimadminlist

列出所有的管理员领地。

/deleteclaim
别名: dc

删除你所在的领地，即使不是你自己的领地。

/deleteallclaims [player]
别名: dac

删除指定玩家的所有领地

/deletealladminclaims
删除所有的管理员领地。

/ignoreclaims
别名: ic

切换忽略领地模式。

/claimclear <target> [<claim> [<world>]]
允许清除一个或者多个领地的实体。

/claimpermissiongroup <group> [<permission> <value>]
别名: cpg

给指定的权限组设定一个可带有注释的权限。

/claimpermissionplayer <player> [<permission> <value>]
别名: cpp

给指定的玩家设定一个可带有注释的权限。

/restorenature
别名: rn

切换至复原（restoration）模式。

/restorenatureaggressive
别名: rna

切换至侵略复原模式（译者注：用于GF插件的入侵模式）。

/restorenaturefill [radius]
别名: rnf

切换至填补（Fill）模式。

/unlockdrops
允许其他玩家拾起你死亡掉落的道具。

/gpdebug <on>|<off>|<record>|<paste> [<player>]
切换调试模式。

/gpreload
重载Grief Prevention的配置设定。
```

#### 聊天

```
/ignoredplayerlist
别名: ignoredlist

查看你的屏蔽名单。

/ignoreplayer [player]
别名: ignore

忽略指定玩家的聊天信息。

/unignoreplayer [player]
别名: unignore

取消对指定玩家的聊天信息的忽略设置。

/separate <player1> <player2>
强制两个玩家无法互相接收消息。

/unseparate <player1> <player2>
关闭指定两个玩家的/separate操作

/softmute <player>
Toggles whether a player's messages will only reach other soft-muted players（译者注：看不懂）。
```

#### 其他

```
/gphelp
列出每一条指令的详细信息。
```

### 选项总览

使用 `/lp user/group <玩家|权限组> meta set <选项> <值> [服务器] [世界]` 改变选项值

选项|默认值|描述
-|-|-
griefprevention.abandon-return-ratio-basic|1.0|当玩家放弃基础领地的时候，领地块的返还倍率。
griefprevention.abandon-return-ratio-town|1.0|当玩家放弃城镇领地的时候，领地块的返还倍率。
griefprevention.blocks-accrued-per-hour|120|每小时赚取的领地块数量，默认情况下，每个活跃状态下的玩家，每五分钟将会获得6个方块。提示：玩家需要在先前记录的位置至少移动三格的距离才算活跃。如果使用'wilderness-cuboids'(3D方式圈地模式，即不按通天的区块计算)模式的话，这个值的默认值将会改变为每小时赚取30720个方块，即每五分钟赚取1536个方块。
griefprevention.claim-create-mode|0|玩家登陆后的默认圈地模式 (0 = 2D, 1 = 3D)
griefprevention.create-claim-limit-basic|20|每个玩家的最大基础领地数 (0 = 不限制)
griefprevention.create-claim-limit-town|1|每个玩家的最大城镇领地数 (0 = 不限制)
griefprevention.create-claim-limit-subdivision|10|每个玩家的最大细分子领地数 (0 = 不限制)
griefprevention.initial-claim-blocks|100|（默认情况下）玩家的初始领地块数量。提示：如果使用'wilderness-cuboids'模式，这个值默认为25600。
griefprevention.max-accrued-claim-blocks|80000|通过在线活跃时间获取的领地块上限。不会影响购买和管理员赠予的数量。提示：如果使用'wilderness-cuboids'模式，这个值默认是20480000.
griefprevention.claim-expiration-chest|7|超过此天数不上线的玩家，放置箱子生成的领地将会被清除。
griefprevention.claim-expiration-basic|14|超过此天数不上线的玩家，基本类型的领地将会被清除。
griefprevention.claim-expiration-town|14|超过此天数不上线的玩家，城镇类型的领地将会被清除。
griefprevention.claim-expiration-subdivision|14|超过此天数不上线的玩家，其领地的子领地将会被清除。
griefprevention.min-claim-level|0/255|领地创建的Y轴高度限制。
griefprevention.max-claim-level|0/255|领地创建的Y轴高度限制。
griefprevention.min-claim-size-basic-x|10/5000|基础领地的最小X轴向延展限制。
griefprevention.max-claim-size-basic-x|10/5000|基础领地的最大X轴向延展限制。
griefprevention.min-claim-size-basic-y|5/256|基础领地的最小Y轴向延展限制。
griefprevention.max-claim-size-basic-y|5/256|基础领地的最大Y轴向延展限制。
griefprevention.min-claim-size-basic-z|10/5000|基础领地的最小Z轴向延展限制。
griefprevention.max-claim-size-basic-z|10/5000|基础领地的最大Z轴向延展限制。
griefprevention.min-claim-size-town-x|32/10000|城镇类型领地的最小X轴向延展限制。
griefprevention.max-claim-size-town-x|32/10000|城镇类型领地的最大X轴向延展限制。
griefprevention.min-claim-size-town-y|32/256|城镇类型领地的最小Y轴向延展限制。
griefprevention.max-claim-size-town-y|32/256|城镇类型领地的最大Y轴向延展限制。
griefprevention.min-claim-size-town-z|32/10000|城镇类型领地的最小Z轴向延展限制。
griefprevention.max-claim-size-town-z|32/10000|城镇类型领地的最大Z轴向延展限制。
griefprevention.max-claim-size-subdivision-x|1000|子领地的最大X轴向延展限制。
griefprevention.max-claim-size-subdivision-y|256|子领地的最大Y轴向延展限制。
griefprevention.max-claim-size-subdivision-z|1000|子领地的最大Z轴向延展限制。

---

## [登录密码]FlexibleLogin

[官方文档](https://github.com/games647/FlexibleLogin) | 
[中文配置](https://github.com/games647/FlexibleLogin/wiki/%E4%B8%AD%E6%96%87----Chinese)

### 安装

下载[最新版本](https://github.com/games647/FlexibleLogin/releases)放入Mod文件夹即可

### 初始化配置文件

#### \config\flexiblelogin\config.conf

`messageInterval=5` 显示未登录信息提示间隔

#### \config\flexiblelogin\locale.conf

复制以下并覆盖

```
# 一个账户已经存在了，因此无法创建用户
accountAlreadyExists {
    color="dark_red"
    text="此账号已经注册过了！"
}
# 当玩家成功注册的时候
accountCreated {
    color="dark_green"
    text="注册成功！"
}
# 当一个账户删除之后
accountDelete {
    arguments {
        account {
            optional=true
        }
    }
    closeArg="}"
    content {
        color=yellow
        extra=[
            {
                text="删除账户："
            },
            {
                text="{account}"
            },
            {
                text="!"
            }
        ]
        text=""
    }
    openArg="{"
    options {
        closeArg="}"
        openArg="{"
    }
}
# 当一个玩家的账户不存在的时候
accountNotFound {
    color="dark_red"
    text="用户不存在！"
}
# 当账户不存在与账户数据库的时候
accountNotLoaded {
    color="dark_green"
    text="您的账户加载异常！"
}
# 如果玩家已经登录过了，当他们再次尝试登陆的时候
alreadyLoggedIn {
    color="dark_red"
    text="你已经登录了！"
}
# 其他玩家想用一个已经在线的玩家名字登陆的时候
alreadyOnline {
    color="dark_green"
    text="您已经在线了！"
}
# 如果玩家成功的修改了他的密码
changePassword {
    color="dark_green"
    text="密码修改成功！"
}
# 如果电子邮件找回密码功能未启用
emailNotEnabled {
    color="dark_green"
    text="未启用电子邮件恢复功能。"
}
# 当玩家使用设置电子邮件指令成功的设置了他的电子邮件的时候
emailSet {
    color="dark_green"
    text="您成功设置了电子邮件。"
}
# 当错误发生的时候 (不该出现)
errorExecutingCommand {
    color="dark_red"
    text="执行指令的过程中发生了异常，请检查控制台。"
}
# 强制注册失败，因为玩家在线
forceRegisterOnline {
    color="dark_green"
    text="无法完成强制注册，此玩家当前在线！"
}
#成功的强制注册了账户
forceRegisterSuccess {
    color="dark_green"
    text="强制注册成功"
}
# 当玩家输入了错误的密码
incorrectPassword {
    color="dark_red"
    text="密码错误！"
}
# 踢出消息：当加入的玩家同已注册的玩家字符相同但大小写不同的时候
invalidCase {
    arguments {
        username {
            optional=true
        }
    }
    closeArg="}"
    content {
        color=yellow
        extra=[
            {
                text="错误的玩家ID。请以ID："
            },
            {
                text="{username}"
            },
            {
                text="加入游戏！"
            }
        ]
        text=""
    }
    openArg="{"
    options {
        closeArg="}"
        openArg="{"
    }
}
# 当玩家以一个Mojang不认可的ID登录的时候
invalidUsername {
    color="dark_red"
    text="无效的用户名 - 字母选择范围是 a-z,A-Z,0-9 并且长度在 2 到 16之间！"
}
# 当玩家通过相同Ip自动登录的时候
ipAutoLogin {
    color="dark_green"
    text="自动登录"
}
# 当一个秘钥被创建的时候
keyGenerated {
    arguments {
        code {
            optional=false
        }
    }
    closeArg="}"
    content {
        color=yellow
        extra=[
            {
                text="秘钥生成成功: "
            },
            {
                text="{code}"
            }
        ]
        text=""
    }
    openArg="{"
    options {
        closeArg="}"
        openArg="{"
    }
}
# 最后一次在线时所记录的时间节点
lastOnline {
    arguments {
        time {
            optional=false
        }
        username {
            optional=true
        }
    }
    closeArg="}"
    content {
        color=yellow
        extra=[
            {
                text="账号: "
            },
            {
                text="{username}"
            },
            {
                text="最后一次在线是"
            },
            {
                text="{time}"
            }
        ]
        text=""
    }
    openArg="{"
    options {
        closeArg="}"
        openArg="{"
    }
}
# 当玩家登陆成功的时候
loggedIn {
    color="dark_green"
    text="登录成功"
}
# 当玩家成功的登出的时候
loggedOut {
    color="dark_green"
    text="登出成功"
}
# 当玩家使用邮件密码找回功能的时候
mailSent {
    color="dark_green"
    text="邮件已发送"
}
# 玩家尝试过多次数的错误密码
maxAttempts {
    color="dark_red"
    text="你输入了太多次的错误密码！请联系管理员"
}
# 玩家到达了Ip注册上限
maxIpReg {
    color="dark_red"
    text="你所使用的IP地址注册太多的账号了，请联系管理员。"
}
# 当玩家输入的Email地址不存在的时候
notEmail {
    color="dark_red"
    text="您输入了一个错误的Email邮件地址!"
}
# 当玩家未登录的时候
notLoggedIn {
    color="dark_red"
    text="未登陆，请输入/login+[密码]登陆游戏。"
}
# 当玩家未注册的时候
notRegistered {
    color="dark_red"
    text="未注册，请输入/register+[密码]+[确认密码]完成注册。"
}
# 管理员重置插件的时候
onReload {
    color="dark_green"
    text="插件重载成功！（汉化By-Tollainmear）"
}
# 当非玩家(例如：控制台、命令方块)尝试执行只有玩家能够执行的指令的时候
playersOnly {
    color="dark_red"
    text="只有玩家可以执行此指令！"
}
# 当玩家执行指令并且`command only protection`启用的时候
protectedCommand {
    color="dark_red"
    text="此指令收到保护，请登录"
}
# 当玩家使用TOTP注册，点击获取可扫描二维码的时候
scanQr {
    color=yellow
    text="点击这里扫描二维码"
}
# 踢出信息：当玩家登录超时的时候
timeoutReason {
    color="dark_red"
    text="登录超时"
}
# 当玩家注册密码过短的时候
tooShortPassword {
    color="dark_green"
    text="您的密码太短了"
}
# 当TOTP不可用的时候
totpNotEnabled {
    color="dark_red"
    text="Totp当前不可用，你必须输入两个密码。"
}
# 当玩家没有或者忘记递交用来找回密码电子邮件地址的时候
uncommittedEmailAddress {
    color="dark_red"
    text="您没有输入任何Email地址！"
}
# 当两次输入的密码不同的时候
unequalPasswords {
    color="dark_red"
    text="两次密码输入的不一样！"
}
# 当未注册玩家处理失败的时候
unregisterFailed {
    color="dark_red"
    text="您的请求不是玩家ID也不是UUID。"
}
# 如果未注册玩家不能登录服务器的时候
unregisteredKick {
    color="dark_green"
    text="您无法连接服务器，请到网页注册。"
}
```

### 权限配置

给与管理员权限组 `flexiblelogin` ，默认玩家权限组`flexiblelogin.command`

```
flexiblelogin.admin - 管理员权限
flexiblelogin.command.login - 使用 /login 命令
flexiblelogin.command.logout - 使用 /logout 命令
flexiblelogin.command.changepw - 使用 /changepassword 命令
flexiblelogin.command.register - 使用 /register 命令
flexiblelogin.command.mail - 使用 /setemail 命令
flexiblelogin.command.forgot - 使用 /forgot 命令
flexiblelogin.no_auto_login - 拥有此权限的玩家无法使用自动登录的功能
flexiblelogin.bypass - 拥有此权限的玩家可以跳过登录
```

### 命令

```
玩家命令:
/reg /register <password> <password> - 使用密码进行注册
/register - 使用TOTP生成密码
/changepw /cp /changepassword <password> <password> - 更改当前密码
/log /l /login <password|code> - 使用密码或验证码登录
/logout - 登出
/mail /setemail - 设置邮箱地址
/forgot /forgotpassword - 发送忘记密码邮件
/unregister <uuid|name> - 删除账户

管理员命令: 
/fl <reload|rl> - 重载配置文件
/fl forcelogin <name> - 强制登录一个玩家
/fl <accounts|acc> <name|ip> - 获取用户账号列表
/fl <unregister|unreg> <name|uuid|--all> - 删除用户
/fl <register|reg> <name|uuid> <pass> - 注册一个用户
/fl <resetpw|resetpassword> <name> - 给用户设置新的密码
```

---

## [创世神]Worldedit

[官网](http://enginehub.org/worldedit/) | 
[中文文档](http://mineplugin.org/WorldEdit#.E5.AE.89.E8.A3.85)

### 安装

下载[最新版本](https://ore.spongepowered.org/EngineHub/WorldEdit/versions)放入Mod文件夹即可

### 权限配置

给与管理员权限组 `worldedit` 权限

### 命令

#### 历史
命令|参数|介绍
-|-|-
//undo|[步数]|撤销你的上一个(或几个)操作
//redo|[步数]|重做你上一个(或几个)被撤销的操作
/clearhistory||清除你的历史记录

#### 选区
命令|参数|介绍
-|-|-
//wand||给予你编辑工具（默认为木斧）使用这个工具左键点击来选择第一个位置，右键点击来选择第二个位置
/toggleeditwand||切换选择工具模式，使你可以正常使用作为选择工具的物品
//sel|<cuboid\extend\poly\ellipsoid\sphere\cyl>|设置选区使用的形状
//desel||清除当前的选区
//pos1||将你站立的方块上方的方块的位置设置为第一个选区位置
//pos2||将你站立的方块上方的方块的位置设置为第二个选区位置
//hpos1||将你指针所指的方块的位置设置为第一个选区位置
//hpos2||将你指针所指的方块的位置设置为第二个选区位置
//expand|<数量>|向你所看的方向扩大选区范围
//expand|<数量> <方向>|向指定方向扩大选区范围 (可用方向有 north,south,east,west,up,down)
//expand|<数量> <反方向数量> [方向]|同时向两个方向扩大选区范围
//expand|vert|将选区扩大到从天空到基岩
//contract|<数量>|向你所看的方向缩小选区范围
//contract|<数量> <方向>|向指定方向缩小选区范围 (可用方向有 north,south,east,west,up,down)
//contract|<数量> <反方向数量> [方向]|同时向两个方向缩小选区范围
//outset|[-hv] <数量>|向所有方向扩大选区范围
//inset|[-hv] <数量>|向所有方向缩小选区范围
//shift|<数量> [方向]|移动选区范围，不移动选区中的内容
//size||得到当前选区的大小
//count|<方块ID>|计算选区内指定方块的数量
//distr|[-c]|计算选区内的方块分布比例

#### 选区操作
命令|参数|介绍
-|-|-
//set|<方块ID>|将选区内的所有方块设定为指定方块
//replace|<到方块ID>|替换所有非空气方块为指定方块
//replace|<从方块ID> <到方块ID>|将所有指定方块替换成另一个指定方块
//overlay|<方块ID>|将指定方块放在选区内所有方块上方
//walls|<方块ID>|用指定方块在选区四周建立墙壁（不包括屋顶与地面）
//outline|<方块ID>|用指定方块在选区周围建立墙壁，屋顶与地面
//smooth|[迭代次数]|平滑化选区的高度图
//deform||按照几何表达式使选区内容变形
//hollow||使选区内部的物体空心
//regen||重新生成选择区域
//move|[数量] [方向] [留存方块ID]|移动选区内容，可以指定一个方块来填充移动后留空的区域
//stack|[数量] [方向]|叠加选区内容
//naturalize||将选区表面3格设定为泥土，下面设定为原石

#### 剪贴板
命令|参数|介绍
-|-|-
//copy||复制当前选区内容，注意你与选区的相对位置将被储存
//cut||剪切当前选区内容
//paste|[-ao]|粘贴剪贴板内容。如果你使用-a标签，空气方块将会被忽略
//rotate|<角度>|旋转剪贴板内容
//flip|[方向]|翻转剪贴板内容
//schematic 或 //schem|save [格式] <文件名>|将剪贴板内容保存为schematic文件（mcedit是目前唯一格式）
//schematic 或 //schem|load [格式] <文件名>|将schematic文件加载到剪贴板
//schematic 或 //schem|list|显示所有schematic文件列表
//schematic 或 //schem|formats|显示所有可用的schematic格式
/clearclipboard||清空你的剪贴板内容

#### 生成
命令|参数|介绍
-|-|-
//generate|<方块ID> <方程>|根据给出的方程生成形状
//hcyl|<方块ID> <半径> [高度]|生成一个竖直的空心圆柱体
//cyl|<方块ID> <半径> [高度]|生成一个竖直的实心圆柱体
//sphere|<方块ID> <半径> [yes(是否生成在上方)]|生成一个球体
//hsphere|<方块ID> <半径> [yes(是否生成在上方)]|生成一个空心球体
//pyramid|<方块ID> <大小>|生成一座金字塔
//hpyramid|<方块ID> <大小>|生成一座空心金字塔
//forestgen|[大小] [种类] [密度]|生成一片森林
//pumpkins|[大小]|生成一片南瓜地

#### 效用性
命令|参数|介绍
-|-|-
/toggleplace||在第一个选择点与你的位置之间切换
//fill|<方块> <半径> [深度]|填充一个洞
//fillr|<方块> <半径>|以递归模式填充完全一个洞
//drain|<半径>|吸干附近的水或岩浆
/fixwater|<半径>|平整附近的水面
/fixlava|<半径>|平整附近的岩浆表面
/removeabove|[大小] [高度]|移除你上方的方块
/removebelow|[大小] [高度]|移除你下方的方块
/replacenear|<大小> <从方块ID> <到方块ID>|替换附近的方块
/removenear|[方块] [范围]|移除附近的方块
/snow|[半径]|模拟降雪
/thaw|[半径]|融化附近的积雪
//ex|[范围]|扑灭附近的火焰
/butcher|[半径]|杀死附近的生物
/remove|<种类> <范围>|清除附近的实体，种类有"items"(物品)，"arrows"(箭)，"boats"(船)，"minecarts(矿车)"，"tnt"或"xp"(经验球)
//green||绿化附近

#### 普命令
命令|参数|介绍
-|-|-
/searchitem||用名字搜索一个物品
/worldedit||WorldEdit命令表
/worldedit help|[命令]|显示给出的命令的介绍，或在没有给出命令时列出所有可用命令(同//help)
//worldedit reload||重新载入WorldEdit配置文件
//worldedit version||显示WorldEdit版本
//worldedit tz||暂时性设置你的时区
//fast||切换高速性能模式

#### 生物群系
命令|参数|介绍
-|-|-
/biome||显示你所在位置的生物群系
/biomelist||显示所有可用的生物群系
/biomeinfo|[-pt]|显示所指方块所在位置的生物群系
//setbiome|[-p] <生物群系>|设置选区为指定生物群系 -p 参数设置你所在位置的生物群系

---

## [前置-变量]PlaceholderAPI

[官网](https://ore.spongepowered.org/rojo8399/PlaceholderAPI) | 
[MCBBS](http://www.mcbbs.net/thread-847941-1-1.html)

### 安装

下载[修复版本](https://github.com/randombyte-developer/PlaceholderAPI/releases)放入Mod文件夹即可

### 权限配置

给与管理员权限组 `placeholderapi` 权限

### 命令

```
/papi - 查看插件版本
/papi list - 列出所有注册的变量符
/papi info {placeholder} - 查看指定变量符的更多信息
/papi parse {player} {placeholder} - 查看指定玩家的变量结果。
/papi reload [placeholder] - 重载指定变量符，没有指定的话会重载整个插件
/papi enable|disable {placeholder} - 开启/关闭指定的变量符
```

---

## [箱子菜单]VirtualChest

[官网](https://github.com/ustc-zzzz/VirtualChest) | 
[中文文档](https://github.com/Tollainmear/VirtualChest-wiki-zh_CN/wiki)

### 安装

下载[最新版本](https://github.com/ustc-zzzz/VirtualChest/releases)放入Mod文件夹即可

### 权限配置

权限节点|描述
-|-
virtualchest|虚拟箱子的所有权限
virtualchest.list|列出菜单清单的权限
virtualchest.reload|重载本插件的权限
virtualchest.bypass|忽略点击频率限制的权限
virtualchest.open|为任何玩家打开任何菜单的权限
virtualchest.open.others|为其他玩家打开菜单的权限
virtualchest.open.self|为自己打开菜单的权限
virtualchest.open.others.example|为其他玩家打开一个名为 example 的菜单的权限
virtualchest.open.self.example|为自己打开名为 example 菜单的权限

### 命令

`/vc reload`  重新加载配置文件

---

## [商店]UniversalMarket

[官网](https://forums.spongepowered.org/t/universal-market-v1-3-1-10-2-1-12-2/22140) | 
[中文文档](http://www.mcbbs.net/thread-792152-1-1.html)

### 安装

下载[汉化版本](http://www.mcbbs.net/thread-792152-1-1.html)，或[最新版本](https://github.com/Xwaffle1/UniversalMarket/releases)放入Mod文件夹即可

### 配置文件初始化

```
Database {
    database-name=market
    ip="1.127.0.0"
    password=password
    port="3306"
    # Use a SQL server that's not saved to a file.
    use-external=false
    username=admin
}
Market {
    # 玩家不能挂售在环球市场上的黑名单列表
    blacklist=[]
    # 玩家挂售的商品是否会过期
    enable-market-expire=true
    # 作为挂售商品的手续费
    market-price=1000
    # 成交商品所需上缴的税率
    market-tax=0.2
    # 是否启用按 market-price 标准收费
    pay-to-sell=false
    # 以价格为基数，尝试上架商品是否收取税费
    tax-to-sell=false
    # 过期时间默认1天 / 24 小时。以毫秒为单位
    time-market-expires=604800000
    # 一个玩家在同一时间能上架几件商品
    total-items-player-can-sell=10
    # 您可以通过给予权限来定义指定玩家可上架的最大上线。例如：'com.xwaffle.universalmarket.addmax.3'允许玩家同时挂售三件商品
    use-permissions-to-sell=false
}
```

### 权限配置

给与默认玩家组以下权限
```
com.xwaffle.universalmarket.open - 打开通用市场GUI
com.xwaffle.universalmarket.add - 使用指令挂售手中道具
```

给与管理员组以下权限
```
com.xwaffle.universalmarket.remove - 移除玩家挂售的商品的权限
```

### 命令

```
/um - 打开UI界面
/um a [价格] [可选的|数量] - 挂售手中道具
```

### 警告

**如果玩家上架商品过期或被管理员移除，但玩家身上背包已满，商品将会消失！**

---

## [全息显示]Holograms

[官网](https://ore.spongepowered.org/RandomByte/Holograms) | 
[中文文档Holograms](http://www.mcbbs.net/thread-660959-1-1.html)| 
[中文文档HologramsPlus](http://www.mcbbs.net/thread-847947-1-1.html)

### 安装

下载[Holograms](https://ore.spongepowered.org/RandomByte/Holograms/versions),[HologramsPlus](https://ore.spongepowered.org/happyzlife/HologramsPlus/versions)放入Mod文件夹即可

### 配置文件初始化

修改\config\hologramsplus.conf 内刷新动态信息时间（秒）

### 警告

如果装有GP领地插件，使用管理员账号在游戏内输入以下命令，防止全息消失
```
/cf entity-spawn minecraft:armor_stand true
/cf entity-chunk-spawn minecraft:armor_stand true
```

### 命令与权限

给与管理员组 `holograms`、`hologramsplus` 权限

指令|缩写|权限|用途
-|-|-|-
/holograms create <文字>|holograms.create|创建单行文字的全息
/holograms createMultiLine <间距> <行数>|/holograms cml ...|holograms.createMultiLine|可以一次性创建多行全息 (推荐间距: 0.3).
/holograms createMultiLine <间距> <文字>|/holograms cml ...|holograms.createMultiLine|可以一次性创建多行全息 （跟上一个有点不同，不用指定行数）这里每一行都可以使用 % 符号来表示，例如: /holograms cml 0.3 行1%行2%行3
/holograms list [maxDistance]|holograms.list|列出距离你半径为10周围的全息图。
/holograms|holograms.list|作用与 /holograms list 一样，但没有限制半径
/hologramsplus select <enable/disable>|hologramsplus.command.select|启用一个全息的动态更新
/hologramsplus refresh|hologramsplus.command.refresh|强制更新所有变量

### 按钮权限与用途

按钮|权限|用途
-|-|-
[TP]|holograms.teleport|将玩家传送到该全息
[CP]|holograms.copy|将全息复制到玩家脚的位置上
[MV]|holograms.move|将全息移动到玩家脚的位置上
[ST]|holograms.setText|直接修改全息的文字，使用此功能前需要先点击[TP]让玩家传送过去才能使用[ST]功能
[TFF]|holograms.setTextFromFile|导入配置文件 config/holograms/input.txt 文本中的文字替换到全息，仅支持单行，可使用颜色字符（不能为ANSI格式，否则乱码）
[DEL]|holograms.delete|删除全息图

---

## [计分板]YYSScoreboard

[官网](http://www.mcbbs.net/thread-837136-1-1.html) | 

### 安装

下载[最新版本](https://github.com/euOnmyoji/YYSScoreboard/releases)放入Mod文件夹即可

### 配置文件初始化

修改\config\yysscoreboard\scoreboard.conf 内对应文字即可

### 权限配置

给与管理员组 `yysscoreboard` 权限，默认玩家组 `yysscoreboard.command.yyssb` 权限

### 命令

命令|备注
-|-
/yyssb reload |重载配置文件
/yyssb on      |玩家个人打开计分板
/yyssb off     |玩家个人关闭计分板
/yyssb toggle |切换打开/关闭状态

---

## [箱子锁]Latch

[官网](https://ore.spongepowered.org/IchorPowered/Latch) | 
[中文文档](http://www.mcbbs.net/thread-786357-1-1.html)

### 安装

下载[最新版本](https://github.com/ichorpowered/latch/releases)放入Mod文件夹即可

### 配置文件初始化

修改\config\latch\latch.conf 内对应文字即可

### 权限配置

给与管理员组 `latch.admin` 权限，默认玩家组 `latch.normal` 权限

### 命令

插件已汉化，输入`lock`查看所有可用命令

---

## [自动公告]PixelAutoMessage

[官网](https://ore.spongepowered.org/FabioZumbi12/PixelAutoMessages) | 
[中文文档](http://www.mcbbs.net/thread-784651-1-1.html)

### 安装

下载[最新版本](https://ore.spongepowered.org/FabioZumbi12/PixelAutoMessages/versions)放入Mod文件夹即可

### 配置文件初始化

修改\config\pixelautomessages\pixelautomessages.conf 内对应文字即可

### 权限配置

给与管理员组 `pam.cmd.reload` 权限

### 命令

```
/pam reload 重新加载配置文件
```

---

## [扫地]SoulClear

[官网](http://www.mcbbs.net/thread-849744-1-1.html) | 

### 安装

下载[最新版本](http://www.mcbbs.net/thread-849744-1-1.html)放入Mod文件夹即可

### 配置文件初始化

修改\config\soulclear\config.yml 内对应文字即可

### 权限配置

给与管理员组 `soulclear` 权限,给与默认玩家权限组 `soulclear.repo` 回购权限

### 命令

```
/soulClear clearHostile  立刻清理敌对生物
/soulClear clearItem  立刻清理地上掉落物
/soulClear repo  打开回购界面
/soulClear reload  重载配置文件
```

---

## [服务器列表]ServerListPlus

[官网](https://ore.spongepowered.org/Minecrell/ServerListPlus) | 

### 安装

下载[最新版本](https://github.com/Minecrell/ServerListPlus/releases)放入Mod文件夹即可

### 配置文件初始化

修改\config\serverlistplus\ServerListPlus.yml 内对应文字即可

### 命令

```
/slp reload 重新载入配置文件
/slp enable 启用功能
```

---

## [多世界管理]ProjectWorlds

[官网](https://ore.spongepowered.org/TrenTech/ProjectWorlds) | 

### 安装

下载[前置](https://ore.spongepowered.org/TrenTech/ProjectCore/versions)、[最新版本](https://ore.spongepowered.org/TrenTech/ProjectWorlds/versions)放入Mod文件夹即可

### 权限配置

给与管理员组 `pjw` 权限

```
pjw.cmd.world.properties
pjw.cmd.world.list
pjw.cmd.world.hardcore
pjw.cmd.world.keepspawnloaded
pjw.cmd.world.setspawn
pjw.cmd.world.difficulty
pjw.cmd.world.regen
pjw.cmd.world.fill
pjw.cmd.world.delete
pjw.cmd.world.create
pjw.cmd.world.teleport
pjw.cmd.world.teleport.others
pjw.cmd.world.gamerule
pjw.cmd.world.rename
pjw.cmd.world.load
pjw.cmd.world.import
pjw.cmd.world.unload
pjw.cmd.world.time
pjw.cmd.world.world
pjw.cmd.world.loadonstartup
pjw.cmd.world.modifier
pjw.cmd.world.usemapfeatures
pjw.cmd.world.border
pjw.cmd.world.border.center
pjw.cmd.world.border.damage
pjw.cmd.world.border.diameter
pjw.cmd.world.border.generate
pjw.cmd.world.border.warning
pjw.cmd.world.border.info
pjw.worlds.<world> - Grant permission to enter world
pjw.override.gamemode - override forced gamemode settings when switching worlds
pjw.override.pvp - override forced pvp settings in worlds
```

### 命令

使用`/helpme <command>`获取命令帮助

```
/world（指令大全）
/world properties（世界属性）
/world create（创造世界，支持创造超平坦）
/world remove（删除世界）
/world regen（重新生成一个世界）
/world difficulty（调整世界的难度）
/world setspawn（设置世界的出生点）
/world keepspawnloaded（保持出生点的区块读取）
/world hardcore（启用hardcore模式？）
/world list（查看世界列表）
/world teleport（传送到那个世界）
/world gamerule（设置世界的游戏规则）
/world unload（取消读取某个世界）
/world load（加载某个世界）
/world import（导入一个世界）
/world rename（更改某个世界的名称）
/world copy（复制某个世界）
/world time（设置世界时间）
/world weather（设置世界气象）
/world loadonstartup（设置世界是否在启动时加载）
/world usemapfeatures（设置世界是否生成村庄等地图特性建筑）
/world modifier （允许添加或删除世界特性，仅生效在未生成的区块）
/world border（地图边界设置，包括中心、伤害、直径、生成、警告、基本信息）
    /world border center
    /world border damage
    /world border diameter
    /world border generate
    /world border warning
    /world border info
```

---

# 箱子菜单设计

##主菜单
1|2|3|4|5|6|7
-|-|-|-|-|-|-
个人信息菜单|基础指令|家菜单|世界市场菜单|领地菜单|箱子锁菜单|世界传送菜单

