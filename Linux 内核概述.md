第1章  Linux内核概述
![](https://cdn.nlark.com/yuque/0/2021/png/366846/1614590241256-701d2772-a5e5-4dfb-8e74-89e2d8263293.png#height=231&id=IF8yn&originHeight=341&originWidth=674&originalType=binary&size=0&status=done&style=none&width=457)![](https://cdn.nlark.com/yuque/0/2021/png/366846/1614590241257-fe9ebb5a-cfbe-4b84-92cc-a99e05b5ec60.png#height=233&id=oRqW9&originHeight=341&originWidth=674&originalType=binary&size=0&status=done&style=none&width=460)
![](https://cdn.nlark.com/yuque/0/2021/png/366846/1614590241297-a821331b-82b8-47b3-a9ba-3533e6c21d31.png#height=234&id=HcSzf&originHeight=341&originWidth=674&originalType=binary&size=0&status=done&style=none&width=463)
![](https://cdn.nlark.com/yuque/0/2021/png/366846/1614590241336-cc4afb63-17a5-4f82-851e-461fec875d0d.png#height=235&id=TA4rz&originHeight=341&originWidth=674&originalType=binary&size=0&status=done&style=none&width=464)
# **1.1 随便说说**
Linux 内核非常庞大，我说的非常大并不是为了吓唬大家，确实是非常多的代码，超过 600 万行的代码，所以我写文章介绍 Linux 内核，也不可能每一行代码去分析，但是我会提炼其中的重点出来，告诉大家，Linux 内核的构成，包含哪些东西，我们不管学习什么，最关键的是学会其中的思想，但是我们如果什么都还不会的时候，可以学着由表透里，就像我打篮球一直不会后仰跳投，但是我可以把科比的研究一遍又一遍，总有一天我也能听到自己打铁的哐哐声。


但是我希望我的文字略带微笑，略带邪恶，微笑着去面对这些代码，去面对 Linux 这片森林，然后你从这个森林走出来后，可以明白森林里的有什么东西，有什么动物，植物，知道哪里有陷阱，怎么利用森林给你的资源让你生存下来，你下一次想带个妹子进去约会，就能找到合适的地方约会。


![image.png](https://cdn.nlark.com/yuque/0/2021/png/366846/1614592052450-167619bb-fd48-436e-9935-170e06062259.png#height=212&id=LOMgX&margin=%5Bobject%20Object%5D&name=image.png&originHeight=560&originWidth=1124&originalType=binary&size=141152&status=done&style=none&width=425)


虽然 Linux 内核非常庞大，但是横向对比计算机的发展史， Linux 的历史非常的简短，计算机出现的时候，还没有什么花里胡哨的包装，都是光着身子跑。计算机只能执行一个任务，执行一个进程，也可以叫「**裸跑**」，学习计算机的同学应该很快明白什么是裸机程序，裸机程序只可以让一个进程使用硬件资源，这无形上对硬件资源的浪费。


拿我们的手机来比喻，裸机的话，我们玩王者荣耀的时候，有人打电话进来的话，王者荣耀是要被终止。后来就有了多任务系统，多任务操作系统可以保证多任务执行，同样的 CPU 芯片，有了操作系统，我可以开微信，QQ ，还同时做其他很多事情。而多任务就是负责协调这些进程工作的。


再后来出现了很多操作系统，操作系统我认为可以**分为实时操作系统和通用操作系统**，实时操作（RTOS 全称Real Time Operating System）系统可以理解为对时间要求非常苛刻，可以用一个词「**必须**」来理解，就是在某个时间片之内必须要做某件事情。


而通用操作系统，我们可以很常见，比如电脑 windows，Android 手机等，Linux 也可以认识是通用 OS,因为他们对时间上要求不是很严苛。


Linux 可以通过配置某个宏定义变成实时操作系统，但是我们使用 Linux 主要是针对他的通用 OS，多用户，多任务等特别突出的性能。


世界上的嵌入式操作系统数不胜数，我有一个很厉害的师弟，前几年也参与了一个国内嵌入式操作系统的开发，当然了，他们是以盈利为目的了，还有我认识的周立功先生，他们公司也研发了一个嵌入式操作系统，但是对于我们开发者来说，做项目时，选择适合自己项目的系统就好了。但对于学习来说，我认为，应该学习最先进的操作系统，这样才能让自己的技术有更大的先进性。




**介绍几个常见的多任务系统**
## **VxWorks**
VxWorks 是美国 WindRiver 公司的产品，市场占有率比较高的嵌入式操作系统。VxWorks 实时操作系统由 400 多个相对独立、短小精悍的目标模块组成，用户可根据需要选择适当的模块来裁剪和配置系统，具有优先级的任务调度、任务间同步与通信、中断处理、定时器和内存管理等功能，符合 POSIX (可移植操作系统接口)规范的内存管理，多处理器控制程序，并且具有简明易懂的用户接口，在核心方面甚至町以微缩到** 8 KB**。


这个操作系统不敢吹太多，可以自己去了解下，因为稳定性太好了，应用到了航空，卫星，军事等各个领域，实用性非常强，所以很多培训机构也针对这个操作系统开了培训课程。


## **μC／OS-II**
μC／OS-II是在μC-OS的基础上发展起来的，是美国嵌入式系统专家 Jean J．Labrosse 用 C 语言编写的一个结构小巧、支持抢占式的多任务实时内核。μC／OS-II 能管理** 64 **个任务，并提供任务调度与管理、内存管理、任务间同步与通信、时间管理和中断服务等功能，具有执行效率高、占用空间小、实时性能优良和可扩展性强等特点。


## **μClinux**
μClinux 是一种优秀的嵌入式 Linux 版本，其全称为 micro-control Linux，从字面意思看是指微控制 Linux。同标准的 Linux 相比，μClinux 的内核非常小，但是它仍然继承了 Linux 操作系统的主要特性，包括良好的稳定性和移植性、强大的网络功能、出色的文件系统支持、标准丰富的 API，以及 TCP／IP 网络协议等。因为没有 MMU 内存管理单元，所以其多任务的实现需要一定技巧。


## ** eCos**
eCos(embedded Configurable operating system)，即嵌入式可配置操作系统。它是一个源代码开放的可配置、可移植、面向深度嵌入式应用的实时操作系统。最大特点是配置灵活，采用模块化设计，核心部分由小同的组件构成，包括内核、C 语言库和底层运行包等。每个组件可提供大量的配置选项(实时内核也可作为可选配置)，使用 eCos 提供的配置工具可以很方便地配置，并通过不同的配置使得 eCos 能够满足不同的嵌入式应用要求。
![](https://cdn.nlark.com/yuque/0/2021/png/366846/1614590241330-e062593a-dc49-405d-894b-c49bf113bcb3.png#height=234&id=bBgFC&originHeight=341&originWidth=674&originalType=binary&size=0&status=done&style=none&width=462)
![](https://cdn.nlark.com/yuque/0/2021/png/366846/1614590241306-93e91f7d-e7e2-48ec-a3be-191cc01554d7.png#height=235&id=R1eG1&originHeight=341&originWidth=674&originalType=binary&size=0&status=done&style=none&width=464)
## **内核的工作**
我们使用的计算机大家都知道是操作系统，**那内核是什么呢？**


那我们先简单说说操作系统，操作系统是面向用户的，计算机用户可以使用计算机操作系统来工作，聊天，玩游戏，**我们使用的这些东西都是应用软件，对应用程序来说，内核就是它的操作系统**，这个系统可以为应用程序工作，管理应用程序。


![image.png](https://cdn.nlark.com/yuque/0/2021/png/366846/1614592890741-b231864d-a54c-480c-a802-067df181a5e9.png#height=262&id=ISRhA&margin=%5Bobject%20Object%5D&name=image.png&originHeight=523&originWidth=523&originalType=binary&size=52529&status=done&style=none&width=261.5)


所以内核还有一个比较重要的工作，**就是管理应用程序的运行**，为应用程序准备好运行内存，管理应用程序的执行，让应用程序通行无阻。在Linux下面我们经常听到别人说nice值，就是用来决定某个应用程序运行优先等级，越优先的应用程序在调度算法中越容易被命中，占领CPU的时间也就越多。当然了，还会存在一些恶劣的情况，恶劣的情况就是导致内存或者资源不够用的情况，应用出现崩溃等异常。


除了管理应用以外，内核还需要管理硬件设备，Linux 内核下面有非常多的设备驱动代码，如果一个内核开发工程师说他不懂设备驱动，那简直就是一个笑话。内核跟 CPU 和硬件设备关系非常密切，在整个操作系统中的地位，具有承上启下的作用。这里的承上启下和HAL中所说的乘上启下存在不同，内核中的驱动需要管理硬件，再返回给内核使用。
# 
# **1.2 UNIX 的诞生**
**生日：**UNIX 在 1969 年出生。
**他的父亲和母亲：** 是 Dennis Ritchie 和 Ken Thompson 两个人擦出了灵感的火花创造出来的。
**出生户籍地址：**贝尔实验室
![](https://cdn.nlark.com/yuque/0/2021/png/366846/1614590241358-d1c1b0a6-c7a2-4166-8631-e0bd2ff66519.png#height=347&id=KpxWP&originHeight=399&originWidth=600&originalType=binary&size=0&status=done&style=none&width=522)
贝尔实验室图片
![](https://cdn.nlark.com/yuque/0/2021/png/366846/1614590241327-f3e29913-3b2d-47a9-8088-61d66ccc99e3.png#height=84&id=ddcpH&originHeight=179&originWidth=600&originalType=binary&size=0&status=done&style=none&width=281)
贝尔实验室的logo
## **出生具体流程：**
1965 年，贝尔实验室要做一个项目，这个项目叫PDP-7计算机计划，发起人是通用电气和麻省理工学院，他们给这个操作系统起了一个漂亮的名字叫做**「MULTICS 操作系统」**（"Multiplexed Information and Computing Service"的缩写）。做事情总是有个计划，他们给这个操作系统给出的计划是，这个操作系统可以多个人使用，按照我们现在的人来说就是多用户系统，多任务，多层次等等。


到了1969 年，发起人觉得这个进度太慢了，本来想早点制造出来我们好用来玩电脑游戏的，结果你们这几个科学家整了这么久还是没整出来，那只好停掉了，停掉了投资方就不再提供后备的资源了，留下的东西就自己瞎整吧，投资方也不管了。


计划被停下来了，当时，Ken Thompson 在调试一个程序，这个程序名字叫做 “星级旅游”，这个程序运行在一个叫做 GE-635 的机器上面，但是因为这个机器的硬件设备比较落后，运行速度非常慢，这让Ken Thompson感觉非常不爽，然后他发现之前做「PDP-7计算机计划」项目的时候还有一台PDP-7计算机，这个计算机就是图片下面的那个计算机，当时应该没有人想到计算机可以做到这么小，然后他们就把 GE-635 程序移植到 PDP-7 计算机上面。


到了1970年，PDP-7 可以运行 GE-635程序了，但是却只能支持两个用户，当时 Brian Kernighan 就开玩笑的称他们的系统是 “UNiplexed Information and Computing Service”，这个缩写就是 UNICS，再后来，大家就取谐音，称为 UNIX。所以1970 年可以称为 UNIX元年。
![](https://cdn.nlark.com/yuque/0/2021/png/366846/1614590241328-fd8982f2-d035-4405-b65d-d449961ec2f7.png#height=143&id=p4ioj&originHeight=143&originWidth=220&originalType=binary&size=0&status=done&style=none&width=220)
汤姆逊和丹尼斯里奇
![](https://cdn.nlark.com/yuque/0/2021/png/366846/1614590241505-f111cabe-af08-442e-9c74-c16d2b293166.png#height=301&id=WteFH&originHeight=580&originWidth=733&originalType=binary&size=0&status=done&style=none&width=381)
PDP-7计算机
# **1.3 自由的产物 BSD**
伯克利软件套件（英语：Berkeley Software Distribution，缩写为 BSD ），也被称为伯克利UNIX（Berkeley UNIX），是一个操作系统的名称。衍生于UNIX（类UNIX），1970年代由伯克利加州大学的学生比尔·乔伊（Bill Joy）开创，也被用来代表其衍生出的各种套件。


BSD 常被当作工作站级别的 UNIX 系统，这得归功于 BSD 用户许可证非常地宽松，许多 1980 年代成立的计算机公司，不少都从 BSD 中获益，比较著名的例子如 DEC 的 Ultrix，以及 Sun 公司的 SunOS。 1990 年代，BSD 很大程度上被 System V 4.x 版以及 OSF/1 系统所取代，但其开源版本被采用，促进了因特网的开发。


BSD 比 Linux 早出现，稳定性和安全性都在 Linux 之上，甚至 Windows 和 OS X 都有来自 BSD 的代码，但是现在一提到开源自由软件，人们首先想到的是Linux，而不是资格更老的BSD？UNIX创始人之一的 Ken Thompson 曾如此评价 Linux，「Linux不过是反微软思潮下的产物」，这个家伙觉得 Linux 不可能有多大的成就，非常自信的觉得 BCD 在任何时候都可以击败 Linux。


但是事实证明，Linux 赢得了这场战争，有实力，也有运气，Linux 在发展的时候，BCD 当时正被官司缠上，没有多余的心思应战 Linux。


一个事情的成功，90% 是由他的领导者决定的，就好像一个球队能走多远，队长和教练可以决定它的深度，Linux 也一样，Linus Torvalds 是位杰出的领袖人物，他成功的让一群性格迥异的、绝非泛泛之辈的黑客共同合作开发，而没有如其他开源项目一般分崩离弃。


还有一点，Linux 的硬件支持比 BSD 好，这在各种终端设备上来说简直就是一种惊喜。


GNU 的大力支持，GNU 的许可证与 BSD 不兼容，因此 Linux 的出现让两者完美结合，所以现在Linux 全名叫 GNU/Linux。


BSD 走的是教堂式的学院派路线，而 Linux 则是代表了市集式的骇客精神；此外还有 Linux 发行版的多样化和商业公司的支持、媒体的偏爱。


BSD原本就有极佳的根基，缺乏的可能是一点运气，未来或许大有可为。
![](https://cdn.nlark.com/yuque/0/2021/png/366846/1614590241360-44cedd7b-5e90-4ab9-b016-4820a93e3366.png#height=228&id=tytnv&originHeight=341&originWidth=674&originalType=binary&size=0&status=done&style=none&width=451)
![](https://cdn.nlark.com/yuque/0/2021/png/366846/1614590241337-58e3ea9d-639f-4668-b6bc-da936ef05f3f.png#height=232&id=R0ILx&originHeight=341&originWidth=674&originalType=binary&size=0&status=done&style=none&width=458)
![](https://cdn.nlark.com/yuque/0/2021/png/366846/1614590241391-d9fa7d0f-d215-4018-9c7b-3c74e304d7de.png#height=260&id=tED9Y&originHeight=367&originWidth=950&originalType=binary&size=0&status=done&style=none&width=674)
# 
# **1.4 GNU计划的产生**
![](https://cdn.nlark.com/yuque/0/2021/png/366846/1614590241378-fda63a5c-f49d-483d-b0db-f58be2ed38b2.png#height=242&id=ka2ky&originHeight=242&originWidth=161&originalType=binary&size=0&status=done&style=none&width=161)
理查·斯托曼


因为 UNIX 操作系统的商业化，原来的 UNXI 系统已经不能再被随意的使用，很多人都希望能有一款免费好用的操作系统。因为并不是每个人都很有钱，也不是每个人都有能力自己去写操作系统，此时，**理查·斯托曼在麻省理工学院人工智能实验室发起 GNU 计划，希望发展出一套完整的开放源代码操作系统来取代 UNIX**，计划中的操作系统，名为 GNU。 


1983年9月27日，理查·斯托曼在 net.UNIX-wizards 和 net.usoft 新闻组中公布这项计划。在此项计划中，开发出了很多我们现在熟悉的常用的工具，包括GNU编译器套装（GCC）、GNU的C库（glibc）、以及 GNU 核心工具组（coreutils）。另外也是 GNU 除错器（GDB）、GNU 二进制实用程序（binutils）的 GNU Cash shell中 和 GNOME 桌面环境。


当然，GNU计划的目的还是开发出一款自由传播的操作系统，这个操作系统的名字叫 Hurd，但是由于对操作系统的要求过高，以至于 Hurd 一直处于测试阶段，本意是一个好事情，但是能力有限啊，开发的东西老是出bug，再好的创意那也是徒劳了。


不过 Linus 大神通过 GNU 发布了自己的  Linux 系统后，就火起来了，真的就一发不可收拾，这也是为什么 GNU 和 Linux 关系密切的原因。我们平时听到GNU/Linux 也是因为这个原因，Linux是开源的产物，但是因为Linux 和 Linus的种种密切关系，也可以认为Linux是Linus的产物。


# **1.5 UNIX 衍生系统发展图**


用文字来描述事实总是感觉有点欠缺，就好像两个人发生争执，可以通过吵嘴解决问题，也可以通过大家解决问题，但是我认为打架应该是最直接的，你说得再多也没有枪杆子来得实在。


本书的重心主要放在 Linux 上，可以观察 Linux 的发展轨迹，还是非常给力的，当然了，BSD 目前来说市场占有率不能跟 Linux 相提并论，但是他在整个 UNIX 上也有有着自己的一席之地的。


![](https://cdn.nlark.com/yuque/0/2021/png/366846/1614590241410-e09b7da6-c49c-47c5-8dd2-23a648bcdcd7.png#height=971&id=o7I2o&originHeight=971&originWidth=1280&originalType=binary&size=0&status=done&style=none&width=1280)
# 
# **1.6 Linux的导火索MINIX**


![](https://cdn.nlark.com/yuque/0/2021/png/366846/1614590241401-d82a9fdd-1242-4d68-9d45-7140999659a3.png#height=242&id=Mv18k&originHeight=326&originWidth=588&originalType=binary&size=0&status=done&style=none&width=436)
MINIX启动界面


在 UNIX 产生后，版权在 AT&T 手里，在 Version 7 UNIX 发布之后，发布新的授权条款，将UNIX 源码私有化，在大学不得再使用 UNIX 源码，荷兰阿姆斯特丹自由大学计算机科学系的塔能鲍姆教授（Andrew Stuart "Andy" Tanenbaum）为了教学,自己写了一个类 UNIX 的小系统，命名为 MINIX（意为mini-UNIX）。


![](https://cdn.nlark.com/yuque/0/2021/png/366846/1614590241386-db85a088-6962-4871-ae35-b8b963ff5cc7.png#height=226&id=nVJ0t&originHeight=480&originWidth=640&originalType=binary&size=0&status=done&style=none&width=301)


永远不要小看任何一个人，如果这个人能够编写出自己的教学操作系统，你更加不要随便惹他，你可能不可以，但是下面的这个家伙是可以的，大家可能都不知道什么是“宏内核”和“微内核”，但是这个家伙和 Linus 的辩论轰动一时，不管怎么说，Linux 应该是现在的胜利者，最直接的原因是开源，让更多的开发者可以使用 Linux 内核移植到自己的设备上，包括 ARM 设备。


但是我们也不能抹杀 ast 的作用，在计算机系统的贡献上，和教学的贡献上，肯定是具有一席之地的，作为本文的撰写者，他们都是我们的始祖，技术无国界，请收下我的膝盖。
# 
# **1.7 Linux 的出生**
![](https://cdn.nlark.com/yuque/0/2021/png/366846/1614590241391-1cf844e5-9a28-43d7-8bcb-90f28d38ca85.png#height=264&id=dhbRO&originHeight=264&originWidth=176&originalType=binary&size=0&status=done&style=none&width=176)
林纳斯 托瓦兹（Linus Torvalds）


我们的主角人物，林纳斯 托瓦兹（Linus Torvalds）1991年，林纳斯·托瓦兹在赫尔辛基大学上学时，对操作系统很好奇。由于但是 **386BSD** 还没有出来。可是他不喜欢他的 386 电脑上的 MS-DOS 操作系统，所以就安装了 Minix，可对 Minix 只允许在教育上使用很不满（在当时 Minix 不允许被用作任何商业使用），于是他便开始写他自己的操作系统。


Linux 的第一个版本在 1991 年 9 月被大学 FTP server 管理员 Ari Lemmke 发布在 Internet上，最初 Torvalds 称这个内核的名称为 「Freax」，意思是自由「free」和奇异「freak」的结合字，并且附上「X」这个常用的字母，以配合所谓的类 UNIX 的系统。但是 FTP 服务器管理员嫌原来的命名「Freax」的名称不好听，把内核的称呼改成「Linux」，当时仅有 10000 行代码，仍必须运行于Minix操作系统之上，而且必须使用硬盘开机，随后在10月份第二个版本（0.02版）发布，同时这位芬兰赫尔辛基的大学生在 comp.os.minix 上发布这样一则公告





| Hello everybody out there using minix- I'm doing a (free) operation system (just a hobby, won't be big and professional like gnu) for 386(486) AT clones. |
| --- |





1994 年 3 月，Linux1.0 版正式发布。为了让 Linux 可以在商业上使用，林纳斯·托瓦兹决定更改他原来的协议（这个协议会限制商业使用），以 GNU GPL 协议来代替。之后许多开发者致力融合 GNU 元素到 Linux 中，做出一个有完整功能的、自由的操作系统。


![](https://cdn.nlark.com/yuque/0/2021/png/366846/1614590241440-07848de4-78e3-453f-baf3-d374909b15f1.png#height=118&id=VTU9l&originHeight=460&originWidth=910&originalType=binary&size=0&status=done&style=none&width=234)
80386 的芯片
![](https://cdn.nlark.com/yuque/0/2021/png/366846/1614590241418-44f790cc-447f-45e9-a9d3-782f13f52864.png#height=139&id=NxRHd&originHeight=375&originWidth=484&originalType=binary&size=0&status=done&style=none&width=180)
80386的电脑


如果单凭林纳斯一个人的力量，Linux 不可能发展到这个程度，我认为在那个时候，他做了一个非常正确的决定，就是「开源」，让世界上更多的优秀程序员加入到他的事业当中，为了让更多的人同步开发，林纳斯还写了 GIT ，这个让很多协作开发者为之兴奋的工具。
# 
# **1.8 Linux的标志物**
![](https://cdn.nlark.com/yuque/0/2021/png/366846/1614590241434-f5b8a0fe-7bf2-491d-bc7c-1b0b94ced1e5.png#height=235&id=OFWxV&originHeight=235&originWidth=196&originalType=binary&size=0&status=done&style=none&width=196)
Linux 的标志和吉祥物是一只名字叫做 Tux 的企鹅，标志的由来是因为 Linus 在澳洲时曾被一只动物园里的企鹅咬了一口，便选择企鹅作为 Linux的标志。更容易被接受的说法是：企鹅代表南极，而南极又是全世界所共有的一块陆地。这也就代表 Linux 是所有人的 Linux。


曾经有一个笑话说林纳斯被企鹅咬了之后，因为咬过的伤口会发炎，发炎的时候伤口会有点疼，晚上写代码想打瞌睡，但是就是因为这个炎症的疼痛感刺激着自己，当然了，这个只是个传说，传说是否是真的，哪天大神心情好了可能会揭晓答案。
![](https://cdn.nlark.com/yuque/0/2021/png/366846/1614590241466-aebd9849-7a85-4004-b246-13dba57ebdd8.png#height=237&id=pnAMO&originHeight=341&originWidth=674&originalType=binary&size=0&status=done&style=none&width=469)


# 
# **1.9 Linux的现状**


今天在 Linus Torvalds 带领下，众多开发共同参与开发和维护 Linux 内核。理查德·斯托曼领导的自由软件基金会，继续提供大量支持 Linux 内核的 GNU 组件。一些个人和企业开发的第三方的非 GNU 组件也提供对 Linux 内核的支持，这些第三方组件包括大量的作品，有内核模块和用户应用程序和库等内容。Linux 社区或企业都推出一些重要的 Linux发行版，包括 Linux内核、GNU组件、非GNU组件，以及其他形式的的软件包管理系统软件。


目前这个阶段，可以说每个人都脱离不开 Linux,好吧，肯定有人跑出来抬杠，我就问你，你手机底层是 Linux 内核你可知道，你说你用的是塞班手机，那里购物的云平台，淘宝，亚马逊等都是用 Linux 开发维护的，好吧你说你是个老板，买东西都是别人给你买的，那么你炒股吧，很多股票平台都是基于 Linux 开发维护的。
![](https://cdn.nlark.com/yuque/0/2021/png/366846/1614590241443-de6eafda-c100-49e8-a4c6-07927c784b4a.png#height=227&id=LFSEK&originHeight=341&originWidth=674&originalType=binary&size=0&status=done&style=none&width=448)
![](https://cdn.nlark.com/yuque/0/2021/png/366846/1614590241457-304691ab-01c9-4512-b567-92b227dc8e3d.png#height=217&id=nO2oi&originHeight=341&originWidth=674&originalType=binary&size=0&status=done&style=none&width=429)


# **1.10 为什么学习Linux**


Linux 内核现在覆盖的领域非常广，手机、平板、路由器等等，就大家非常喜欢的苹果操作系统，底层内核也是有 Linux的影子，Linux 的普及毋庸置疑，学习 Linux 应该作为每个技术人员的标配。


刚开始参加工作的时候，很多面试官都问我，你对 Linux 是不是非常懂，我每次都回答，我对 Linux 也是刚刚入门，但是我非常喜欢Linux ，而且我也会持续在 Linux 上面做研究，学习，我也喜欢和这些 Linux 爱好者一起探讨问题。


学习完 Linux 内核你会对整个计算机体系有一个更深刻的认知，作为一个开发者，不管你从事的是驱动开发，应用开发，还是后台开发，你都需要理解计算机操作系统和内核的运行机制，才可能更好的编写你的代码，出现更少的错误。


作为开发人员，不应该只局限在自己的小领域，因为你设计的模块，看起来非常小，但是你不了解进程的调用机制，不知道进程会阻塞，就绪，执行几个状态，你怎么可能编写好一个低容错率的代码呢？
![](https://cdn.nlark.com/yuque/0/2021/png/366846/1614590241418-c2138d0b-1596-4d7c-9237-2643bab71d35.png#height=242&id=qM4Rv&originHeight=341&originWidth=674&originalType=binary&size=0&status=done&style=none&width=478)
