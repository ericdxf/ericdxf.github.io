### 将Nginx注册为windows系统服务

- 在项目主页内找到winsw的下载页面（我用的是最新2.0.2版本，你可以下载最新的）

http://repo.jenkins-ci.org/releases/com/sun/winsw/winsw/2.0.2/

下载的文件：winsw-2.9.0-bin.exe

- nginx项目页面下载nginx windows版本

http://nginx.org

把nginx压缩包解压放到指定目录，例如我放的是d盘根目录。特别强调路径不要带空格的，否则会启动失败。

nginx安装目录是：`E:\myProgram\nginx-1.14.0`

- 将winsw-2.9.0-bin.exe复制到nginx目录：E:\myProgram\nginx-1.14.0，并将其改成nginx-service.exe 

- 新建一个xml文件nginx-service.xml，名称一定要与上面的.exe上的文件名一致的哦。文件内容如下：

```
<service>
  <id>nginx</id>
  <name>nginx</name>
  <description>nginx</description>
  <env name="path" value="E:\myProgram\nginx-1.14.0"/>
  <executable>E:/myProgram/nginx-1.14.0/nginx.exe</executable>
  <arguments>-p E:/myProgram/nginx-1.14.0</arguments>
  <logpath>E:/myProgram/nginx-1.14.0/logs/</logpath>
  <logmode>roll</logmode>
</service>
```

- 运行Windows cmd命令，进入nginx目录：运行nginx-service.exe install将其注册为windws系统服务。当配置错误（就是系统服务中有了但是启动不了）或者是要卸载它的时候运行：nginx-service.exe uninstall

​		再运行Windows cmd命令，输入services.msc,就可以在系统服务中看到nginx服务，右击启动就可以了，访问http://localhost:8088出现nginx页面，安装成功。 因为我这台电脑80端口被占用了，然后就使用了8088端口

