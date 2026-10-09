# Hertz的国家拓展mod（暂定此名）

## 一、相关目标
### 目标一：完成“叫门天子不会爱上大和家公主”

#### 目前已经完成：
    ·创建天明tag(TMG)
    ·国家旗帜暂定为 燕明国家娘
    ·测试用的变身决议
    ·国家理念用的大西理念做占位
    ·测试版本地化
    ·事件链第一个决议和入线事件

#### 需要完成：
    ·countries/TenMing.txt的名称细化
    ·ideas/hertz_ideas.txt天明理念定制化
    ·朱祁镇逃亡事件链
    ·天明风味事件和任务树

#### 可选目标：
    ·寻找更适配的国家旗帜

## 二、具体实现
### 2.1 朱祁镇逃亡事件链：
#### 2.1.1 思路整理
    先整理整理原版的瓦剌风味事件
    ·朱祁镇亲征flavor_oir.1
    ·瓦剌视角朱祁镇被抓flavor_oir.2
    ·明视角朱祁镇被抓flavor_oir.3
    ·明另立新君flavor_oir.4
    ·瓦剌占领北京flavor_oir.5

    应从朱祁镇被抓处修改：
    大明方面：
    ·玩家明获得第二个选项，选择后在*天内可以触发*个事件，用于确定朱祁镇军的落脚点（暂时只做平壤落脚，或许后面可做对马岛、北海道、四国岛、琉球、台湾、大越、文莱、巽他、斯里兰卡、澳大利亚、北美洲、中美洲、非洲）
    ·玩家明第二个选项后在几个月内触发多个事件描述朱祁镇逃亡过程，包括选择逃亡方向的事件
    整理一些重要FLAG：
    ·captured_mng_emperor	flavor_oir.2（OIR 设置）	flavor_oir.4、flavor_oir.5
    ·mng_emperor_captured	flavor_oir.2（MNG 设置）	flavor_oir.4、flavor_oir.6、flavor_oir.7

#### 2.1.2 实现思路：
    "
    1.瓦剌抓到朱祁镇改为触发flavor_oir.3，ai选择选项一后，瓦剌收到flavor_oir.2
    2.新建option为选项二，添加FLAG1朱祁镇逃亡，十天内触发决策逃亡方向（目前刚把第二个选项的代码写出来）
    "已经推翻，兼容性太低
    新思路：附加一个on_action和event来控制天明事件，并写进大明任务树
    1.添加一个hertz_on_actions.txt来上flag（hertz_flag_istoTMG）
    2.给大明写一个决议（hertz_MNGtoTMG_startdecision）
    用于检测hertz_flag_istoTMG是否存在，如果存在可以完成决议，触发hertz_TMG_startselection，之后选择确定的选项将删除captured_mng_emperor和mng_emperor_captured
    3.写好hertz_TMG_startselection这个event
    选项a.守住北京，天子当归！
    选项b.就此放逐，永除后患！（天明线）
    flag:hertz_TMG_had_startselection用于取消决议显示
    4.写好了目前的本地化并进行了测试，测试全通