## UNIX/Linux是多用户系统
* 主机连接多台字符终端
* 字符终端作为交互输入输出设备
## 终端的构成
* 键盘
* 显示器
* RS232串行通信接口
## 主机与终端的连接
* 主机中的串口卡（硬件）引出多个RS232串口
* 每个RS232接口通过电缆连接一台终端
## 终端和主机的功能分工
* 终端是主机的输入和输出设备
* 主机完成程序和数据的存储和处理
## 行律与驱动程序
* 不同的硬件需要不同的驱动程序，驱动程序有与行律模块的接口，上行和下行字符流
* 行律负责一行内字符的缓冲、回显和编辑，直到按下回车键。负责数据加工，如将\n转化为\r\n。将CTRL-C字符转化为中止进程运行的信号
## 行律功能的调整
* 程序中通过编程的方法
* 相关命令stty
`stty erase ^H`、`stty -a`
## 转义字符
* Esc：ASCII码1B、十进制27、八进制033
* 主机发往终端方向的数据中，转义序列的功能有：
1. 控制光标位置、字符颜色、字符大小
2. 选择终端的字符集
3. 控制终端上的打印机、刷卡机、磁条机、密码键盘
* `Esc[2J` 1B 5B 32 4A 清除屏幕
* `Esc[8A` 光标上移8行
* `Esc[16;8H` 光标移到16行8列
* `Esc[1;31m` 红色字符
## 终端类型
* 定义一组转义序列及对应的操作
* ansi vt100 vt220
* 主机根据终端类型，实现相应功能时发送对应的控制码；当终端类型设置的不对，可能一些全屏幕操作的软件运行失败
## 仿真终端
* PC机和RS232串口，运行终端仿真软件来仿一个真正的终端设备的功能
* 仿真的内容包括实现终端的转义码序列功能
## 虚拟终端
* 终端与主机之间的通信由串口线替代为一个TCP连接，双向传递字节流
* 主机与PC通过网络相连，客户端运行telnet，服务端运行telnetd，成为Linux的基于TCP通信的虚拟终端
* 安全终端，在TCP连接上加密和压缩数据，如：Windows客户端软件SecureCRT或者putty
## 普通用户和超级用户
* 由root用户使用useradd命令创建，用户信息存在/etc/passwd文件中，包括用户名和用户ID，以及Home目录，登录shell
* 登录shell：一般为bash，也可以选择其他shell
## man查询联机手册
* `man name`
* `man section name` 章节编号：1 命令，2 系统调用，3 库函数，5 配置文件
* `man -k regexp` 列出关键字与正则表达式regexp匹配的手册项目录
* `q` 退出
## date 读取系统日期和时间
* `date` 读取系统日期和时间
* `date "+%Y-%m-%d %H:%M:%S Day %j"` 定制输出格式，第一个字符是+
* `date "+%s"` 秒坐标，从UTC1970开始，用于计算时间间隔
* 通过NTP协议校对系统时间：命令ntpdate
* `ntpdate 0.pool.ntp.org` 设置时间，必须是root用户
* `ntpdate -q 0.pool.ntp.org` 查询时间，可以是普通用户
## cal 打印日历
* `cal` 打印当前月份的日历
* `cal 2020` 打印2020年的日历
* `cal 10 2019` 打印2019年10月的日历
* `cal 12` 打印公元12年的日历
## bc 计算器
* `bc` 缺省精度为小数点后0位
* `bc -l` 缺省精度位小数点后20位
* `scale=10000` 可以通过scale自行决定精度（小数点位数）
## passwd 更换口令
* `passwd liu` root可以强迫设置其他用户口令
## 口令的设置与验证
* 不存明码口令
* 无法由哈希值倒推口令原文
## who 确定有谁在系统中
* `who` 列出当前已登入系统的用户
* 第一列：用户名，第二列：终端设备的设备文件名
* 设备在文件系统中有一个文件名，设备文件一般放在/dev下
* `tty` 当前终端的设备文件名
* `who am i` 列出当前终端上的登录用户
* `whoami` 仅列出当前终端上的登录用户名
## uptime 已开机时间
* 系统启动到现在的运行时间
* 当前登入系统的用户数
* 近期1分钟、5分钟、15分钟内系统CPU的负载
## top 列出资源占用排名靠前的进程
* VIRT：进程逻辑地址空间大小
* RES：驻留内存数，占用物理内存数
* SHR：与其他进程共享的内存数
* %CPU：占用CPU的百分比
* %MEM：占用内存百分比
* TIME+： 占用CPU的时间
## ps 查询进程状态
* `ps` 只列出当前终端上启动的进程
* `ps -e` 列出系统中所有进程
* `ps -f` 以full格式列出每一个进程
* `ps -l` 以long格式列出每一个进程
* UID: 用户ID、PID：进程ID、PPID：父进程的PID
* C：CPU占用指数、STIME：启动时间、SZ：进程逻辑内存大小、TIME：累计执行时间（占用CPU的时间）
* TTY：终端的名字
* CMD：命令名
* PRI：优先级
* S：状态，S（Sleep）、R（Run）、Z（Zombie）
## free 内存使用情况
* Linux为提高效率，利用程序暂时不用的内存，缓冲读写过的磁盘信息
## vmstat 了解系统负载
* procs：r等待运行的进程数、b处在非中断睡眠状态的进程数
* memory：freek空闲内存，buff/cache用作缓存的内存数
* swap：磁盘/内存的交换页数 KB/s
* io：块设备I/O块数 块/s
* system：in：每秒的硬件中断数，cs：每秒的环境切换次数
* cpu：总使用率，us=user，id=idle，wa=wait for disk I/O