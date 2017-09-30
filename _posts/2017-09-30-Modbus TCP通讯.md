---
layout: post
title: "Modbus TCP通讯"
date: 2017-09-30 
description: "电气"
tag: 电气
---
因为项目中要用到LED显示屏，其支持modbus TCP通讯协议，因为使用的是ET200SP的PLC，所以想采用modbus TCP 进行通讯，具体操作方法如下

### 新建项目

采用博途V14SP1软件新建项目![image_1br5lo01dubemaa1epj1g7711pf9.png-169.2kB][1]
在`属性`选项点击`常规`里进行IP地址的设置

### 建立程序块

打开`组织块`添加`MB_CLIENT`指令，我使用的是V4.1版本的。软件将提示会为该FB 块增加一个背景数据块
![image_1br5m0fp3100i77m1m7f1nomocfm.png-177.8kB][2]
然后添加一个全局数据块用于匹配功能块`MB_CLIENT`的管脚参数`CONNECT`，本人建立的是`MB_CLIENT_TCON.TCON`DB2数据块，然后打开该数据块，手动输入`TCON_IP_v4`的数据类
型![image_1br5mdkd1qik1ojq40qhbr10a813.png-201kB][3]
其中各参数的数据类型如下：![image_1br5mh6h413ukjv6o36pefsrm1g.png-141.1kB][4]
其中需注意的是一个`ID`数值需要与其指令中的背景数据块里面的`MB_UNIT_ID`一致才行,不然通信会出错，当时通信时没注意，搞了好久服务器的IP地址也要设对，不然也连不上。![image_1br5ml9rp1etge121p5c17u6k3a1t.png-230.2kB][5]

### 创建非优化块

创建一个全局数据块用于匹配功能块`MB_CLIENT`的管脚参数`MB_DATA_PTR`，这个数据块是非优化的数据块。点击数据块在其属性里将`优化的块访问`选项点掉,创建完后点击`编译`按钮编译下，才能看到其偏移地址。建个数组用于存储数据![image_1br5n19bmofs39hb031fnalok2a.png-19.1kB][6]
![image_1br5n5be613drma213aps9p1aee2n.png-97.3kB][7]

### 其他管脚数据

新建变量表，将管脚数据依次定义下，或者新建数据块，将管脚定义下，我用的是数据块。
![image_1br5ngvcd1qeq1321145h1voh1bc93k.png-157.6kB][8]
![image_1br5nqrr012pavnf2nrv661h0p4h.png-59.3kB][9]

### 功能块进行匹配

![image_1br5nu5lg4891snkpvl1r0svoe4u.png-27.3kB][10]
其中需注意的是，偏移地址手动输上去，这样和设置的一样

### 连接调试

下载到PLC再线监控，没连上时`status`显示7002，说明没联系上，连上后为7004，个功能码意思在博途指令说明里有详细的介绍
![image_1br5o4fe11gjjsha1fp1hj6qv55b.png-30.3kB][11]
![image_1br5o7htalfo43pgd4lre1dqi5o.png-63.4kB][12]
新建监控表进行监控，将reg改为1，通信`status`一直在`0000-7005-7006`间闪烁
![image_1br5o9lak1ks91fcr56c1quh1hfj65.png-72.3kB][13]
可以采用modbus slave测试软件进行测试，连接后，其中的ID需与PLC中的ID一样，才可传输数据。
![image_1br5udddarnf6r31t8erfq1o6m7f.png-82.4kB][14]

### 注意和疑惑

- 连上后显示`7004`更改ID和IP地址，还是能通讯，说明连上后，更改参数是不起作用的，
- 更改服务器IP地址PLC需重新上电后，可连接其他服务器
- 服务器中断后`status`显示`7002`，说明没连上，连上显示`7004`
- modbus寄存器地址和`DATA_ADDR`是一一对应的，具体对应表，对应的不同读写的功能，在功能块的说明里有详细的对照表
- 同时连接2个服务器，说明示需要独立的client块，独立的背景数据块和ID号，以及IP地址，还没试
[1]: http://static.zybuluo.com/haozhihao/7je7vaehj6ymeinldbb58x3r/image_1br5lo01dubemaa1epj1g7711pf9.png
  [2]: http://static.zybuluo.com/haozhihao/69pxkgsqrzgukyiarehp8jhg/image_1br5m0fp3100i77m1m7f1nomocfm.png
  [3]: http://static.zybuluo.com/haozhihao/uzdwsbqobe5lcvwcqklj5zde/image_1br5mdkd1qik1ojq40qhbr10a813.png
  [4]: http://static.zybuluo.com/haozhihao/9nw319czr60rn0lvwfeym6b8/image_1br5mh6h413ukjv6o36pefsrm1g.png
  [5]: http://static.zybuluo.com/haozhihao/z89umnn4u2bdkqj8tt9nqadc/image_1br5ml9rp1etge121p5c17u6k3a1t.png
  [6]: http://static.zybuluo.com/haozhihao/tswsnlytkocrps57y6bxq8uo/image_1br5n19bmofs39hb031fnalok2a.png
  [7]: http://static.zybuluo.com/haozhihao/xvlwhh2moxfvlftppkonhpx2/image_1br5n5be613drma213aps9p1aee2n.png
  [8]: http://static.zybuluo.com/haozhihao/ow97kt0oisd1fvyjra4zcnhu/image_1br5ngvcd1qeq1321145h1voh1bc93k.png
  [9]: http://static.zybuluo.com/haozhihao/1pkw0squ9txwzes2ai498qtd/image_1br5nqrr012pavnf2nrv661h0p4h.png
  [10]: http://static.zybuluo.com/haozhihao/tphw15wst4hc4xqc36yrxmoj/image_1br5nu5lg4891snkpvl1r0svoe4u.png
  [11]: http://static.zybuluo.com/haozhihao/op1k4jq0rndni3nee8iatssh/image_1br5o4fe11gjjsha1fp1hj6qv55b.png
  [12]: http://static.zybuluo.com/haozhihao/huyc6dfs40h72ic5eq5mhjue/image_1br5o7htalfo43pgd4lre1dqi5o.png
  [13]: http://static.zybuluo.com/haozhihao/rz1nhfdqar1voo9xz0f1edad/image_1br5o9lak1ks91fcr56c1quh1hfj65.png
  [14]: http://static.zybuluo.com/haozhihao/gxt5zu5v04bn4ozzqqu8tpv3/image_1br5udddarnf6r31t8erfq1o6m7f.png