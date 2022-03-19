创建vbs文件
```bash
set ws=WScript.CreateObject("WScript.Shell")
ws.Run "E:\bin\beancount.bat",0
```

创建vbs文件的快捷方式，放到启动目录
```bash
# win+r
shell:startup
```

这样就能开机自启bat脚本了