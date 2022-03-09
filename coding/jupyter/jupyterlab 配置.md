# 安装
```bash
pip3 install jupyterlab
```

# 生成配置文件及密码
```bash
jupyter lab --generate-config 
ipython
In [1]: from notebook.auth import passwd
In [2]: passwd()
Out[2]: 'argon2:$argon2id$v=19$m=10240,t=10,p=8$McKErBf9enzhsvSnwLdU4g$VELawmya+VPaR9w1juYauGW2jqBUEPMES2AtqyhmDgs'
# 这个密文复制下来，粘贴进配置文件
```

# 修改配置文件
```bash
vi /root/.jupyter/jupyter_lab_config.py

c.NotebookApp.ip = '*'
c.NotebookApp.password = u'argon2:$argon2id$v=19$m=10240,t=10,p=8$McKErBf9enzhsvSnwLdU4g$VELawmya+VPaR9w1juYauGW2jqBUEPMES2AtqyhmDgs'
c.NotebookApp.open_browser = False
c.NotebookApp.port = 8888
c.NotebookApp.notebook_dir = '/media/disk/service/jupyterlab'
c.NotebookApp.allow_remote_access = True

# 注意防火墙开启 8888 端口
```

# 使用 `screen` 会话永久开启 `jupyterlab` 服务
```bash
# 开启一个名称为jupyterlab的会话 
screen -S jupyterlab

# 开启jupyterlab服务
jupyter lab --allow-root

# 快捷键 Ctrl + A + D 退出会话窗口，服务仍然运行
# 查看所有会话
screen -ls
```

# 安装中文插件
```bash
pip3 install jupyterlab-language-pack-zh-CN
```
然后刷新浏览器， `setting` -> `language` -> `中文`
