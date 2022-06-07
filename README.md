# 某塔 去除登录绑定手机号 .so 文件  
  
  
把 宝塔官方的 .so 文件 删除：libAuth.aarch64.so libAuth.glibc-2.14.x86_64.so libAuth.loongarch64.so libAuth.x86-64.so libAuth.x86.so pluginAuth.cpython-37m-aarch64-linux-gnu.so pluginAuth.cpython-37m-i386-linux-gnu.so pluginAuth.cpython-37m-loongarch64-linux-gnu.so pluginAuth.cpython-37m-x86_64-linux-gnu.so pluginAuth.cpython-310-aarch64-linux-gnu.so pluginAuth.so  
  
把下载下来的 panelPlugin.py panelSSL.py pluginAuth.cpython-37m-x86_64-linux-gnu.so pluginAuth.so  四个文件 替换搭配 /www/server/panel/class 目录下面
  
pluginAuth.cpython-37m-x86_64-linux-gnu.so pluginAuth.so 因为编译了，暂时不开源，防止泛滥，如果担心安全可以关闭本GitHub项目，懂代码的大佬可以自己拖到 IDA 查看代码！  
  
# 原理
1. panelSSL.py 第 1042 行 rtmp = public.httpPost('http://www.example.com/api'+'/GetToken',pdata)   替换成 你的伪登录token接口
2. panelPlugin.py 第 38 行 #_check_url=__api_root_url+'/panel/get_soft_list_status'     #检测云端状态的注释掉（上传的文件已经注释掉了） 
3. panelPlugin.py 第 1304 行 #public.run_thread(self.is_verify_unbinding,args=(get,))      #每次加载列表 会 检测账户绑定 ！（上传的文件已经注释掉了）
### 就这么简单 没什么 东西 主要 就是 pluginAuth 文件 加密列表  加密插件（因为用不到 插件安装 都是直接上传 已经下载好的解密插件，所以这个功能就没写）  
  
# 部署方法
