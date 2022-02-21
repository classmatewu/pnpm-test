# 你可能不知道的 pnpm 破局之道
:hammer:pnpm first experience  

### 一、关于硬链接(Hard Link)、软链接(Symbolic Link)、节点索引(inode)

0. 节点索引 inode  
在 Linux 系统中，任何一个文件都有对应的inode值，是文件的引用地址

1. 硬链接  
硬链接其实是源文件的副本  
硬链接文件是指向源文件的inode值，源文件的位置位置发生改变，其inode值也会跟着发生改变，所以其硬链接文件也会跟着变，它俩是同一个inode值。  
硬链接只能链接文件，而不能链接文件夹  
硬链接只能链接同卷文件，而不能链接跨卷

2. 软连接  
软链接是源文件的一种快捷方式  
软连接文件指向的是源文件本身，其inode值是不变的，所以当源文件位置发生变化时，源文件的inode值发生变化了，所以软连接也就跟着失效了  
软链接既能链接文件，也能链接文件夹  
软连接既能链接同卷文件，也能链接跨卷文件  

3. 不管是硬链接还是软链接，只要改变了任何一方的内容，另一方的内容也会跟着发生改变

4. 相关的命令  
`mkdir` 创建文件夹  
`touch` 创建文件  
`ln test test_hard` 为test文件创建一个名为test_hard的硬链接文件  
`ln -s test test_soft` 为test文件创建一个名为test_soft的软连接文件  
`ls` 查看当前路径的文件  
`ls -i` 查看当前路径的文件和inode值  
`less` 查看文件内容

### 二、node 引用文件的几种方式
1. 核心模块
2. 官方模块
3. 第三方库

### 三、pnpm 要解决的问题
1. 依赖的存储空间优化：真实文件统一磁盘位置存储，项目中利用虚拟目录进行硬链接
2. 安装依赖的速度提升：由于存储空间的优化，所以一些依赖文件不用重复安装，甚至多版本依赖也只需要追加差异文件即可。
3. 幽灵依赖存在的安全风险：通过`软链接 + 硬链接`的方式，软链接到的目录严格按照依赖定义，硬链接到的文件所在的目录是一个扁平化的依赖结构。同时硬链接目录通过`<package-name>@<version>`来实现相同模块不同版本之间隔离和复用。

### 四、如何解决并且兼容 Node 的依赖引用方式


### 五、参考文档  
[pnpm 中文网](https://www.pnpm.cn/motivation)  
[Hard links and Symbolic links — A comparison](https://medium.com/@307/hard-links-and-symbolic-links-a-comparison-7f2b56864cdd)  
[关于现代包管理器的深度思考——为什么现在我更推荐 pnpm 而不是 npm/yarn?](https://juejin.cn/post/6932046455733485575)  
[都2022年了，pnpm快到碗里来！](https://juejin.cn/post/7053340250210795557)  
