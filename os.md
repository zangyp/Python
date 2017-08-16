# os
os模块用于处理文件和目录
***
### os.getcwd() 
用于返回当前工作目录
***
### os.mkdir(path[, mode])
用于以数字权限模式创建目录，默认的模式为 0777 (八进制)
* path--要创建的目录
* mode--要为目录设置的权限数字模式
***
### os.chmod(path,mode)
用于更改文件或目录的权限
* path--文件名路径或目录路径
* mode--可用以下选项按位或操作生成，
目录的读权限表示可以获取目录里文件名列表，
执行权限表示可以把工作目录切换到此目录，
删除添加目录里的文件必须同时有写和执行权限，
文件权限以用户id->组id->其它顺序检验,最先匹配的允许或禁止权限被应用
  * stat.S_IXOTH: 其他用户有执行权0o001
  * stat.S_IWOTH: 其他用户有写权限0o002
  * stat.S_IROTH: 其他用户有读权限0o004
  * stat.S_IRWXO: 其他用户有全部权限(权限掩码)0o007
  * stat.S_IXGRP: 组用户有执行权限0o010
  * stat.S_IWGRP: 组用户有写权限0o020
  * stat.S_IRGRP: 组用户有读权限0o040
  * stat.S_IRWXG: 组用户有全部权限(权限掩码)0o070
  * stat.S_IXUSR: 拥有者具有执行权限0o100
  * stat.S_IWUSR: 拥有者具有写权限0o200
  * stat.S_IRUSR: 拥有者具有读权限0o400
  * stat.S_IRWXU: 拥有者有全部权限(权限掩码)0o700
  * stat.S_ISVTX: 目录里文件目录只有拥有者才可删除更改0o1000
  * stat.S_ISGID: 执行此文件其进程有效组为文件所在组0o2000
  * stat.S_ISUID: 执行此文件其进程有效用户为文件所有者0o4000
  * stat.S_IREAD: windows下设为只读
  * stat.S_IWRITE: windows下取消只读
***
### os.chdir(path) 
用于改变当前工作目录到指定的路径
* path--要切换到的新路径
***
### os.rmdir(path)
用于删除指定路径的目录，仅当这文件夹是空的才可以，否则抛出OSError
* path--要删除的目录路径
***
### os.remove()
用于删除指定路径的文件，如果指定的路径是一个目录，将抛出OSError，在Unix, Windows中有效
* path--要移除的文件路径
***
### os.path.join(path1[, path2[, ...]])
把目录和文件名合成一个路径
* path--要合成的路径名或文件名
***
