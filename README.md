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
1. 先装一个 宝塔面板 然后装好环境， 然后 创建1个站点 - 	example.com / www.example.com （必须填这个域名 用来hosts 重定向的 pluginAuth.so 列表里的 域名是这个）  
2. example.com / www.example.com 站点 301 重定向 到  自己的域名 
3. 创建自己域名的站点 - 绑定自己的域名 例如： domian.com / www.domian.com 申请ssl 导入伪静态 如下：  
```
if (!-d $request_filename){
	set $rule_0 1$rule_0;
}
if (!-f $request_filename){
	set $rule_0 2$rule_0;
}
if ($rule_0 = "21"){
  # 列表
	rewrite ^/(panel/get_plugin_list)$ /panel/get_plugin_list.json?s=/$1 last;
	# 登录
	rewrite ^/(api/GetToken)$ /api/token.json?s=/$1 last;
	rewrite ^/(.*)$ /index.php/$1;
}

```
4. 自己域名站点下目录 创建 panel 和 api 文件 把 get_plugin_list.json文件 放到panel文件里，token.json文件 放到api文件里 然后访问 http://domian.com/panel/get_plugin_list / http://domian.com/api/GetToken 看看 能不能访问！（这两个文件在 项目 data 目录里下载 ）  
5. 最后确保以上都操作对了，然后把 宝塔面板里 /www/server/panel/class 目录下面 官方的so 全部删除：libAuth.aarch64.so libAuth.glibc-2.14.x86_64.so libAuth.loongarch64.so libAuth.x86-64.so libAuth.x86.so pluginAuth.cpython-37m-aarch64-linux-gnu.so pluginAuth.cpython-37m-i386-linux-gnu.so pluginAuth.cpython-37m-loongarch64-linux-gnu.so pluginAuth.cpython-37m-x86_64-linux-gnu.so pluginAuth.cpython-310-aarch64-linux-gnu.so pluginAuth.so  
6. 然后 下载项目里的 panelPlugin.py panelSSL.py pluginAuth.cpython-37m-x86_64-linux-gnu.so pluginAuth.so  四个文件 放进去 把 panelSSL.py 里 第 1042 行 rtmp = public.httpPost('http://www.example.com/api'+'/GetToken',pdata)   替换成 你的伪登录token接口 例如：rtmp = public.httpPost('http://www.domian.com/api'+'/GetToken',pdata)
