某塔 去除登录绑定手机号 .so 文件  
  
  
把 宝塔官方的 .so 文件 删除：libAuth.aarch64.so libAuth.glibc-2.14.x86_64.so libAuth.loongarch64.so libAuth.x86-64.so libAuth.x86.so pluginAuth.cpython-37m-aarch64-linux-gnu.so pluginAuth.cpython-37m-i386-linux-gnu.so pluginAuth.cpython-37m-loongarch64-linux-gnu.so pluginAuth.cpython-37m-x86_64-linux-gnu.so pluginAuth.cpython-310-aarch64-linux-gnu.so pluginAuth.so  
  
把下载下来的 panelPlugin.py panelSSL.py pluginAuth.cpython-37m-x86_64-linux-gnu.so pluginAuth.so  四个文件 替换搭配 /www/server/panel/class 目录下面
  
pluginAuth.cpython-37m-x86_64-linux-gnu.so pluginAuth.so 因为编译了，暂时不开源，防止泛滥，如果担心安全可以关闭本GitHub项目，懂代码的大佬可以自己拖到 IDA 查看代码！  
  
#123
