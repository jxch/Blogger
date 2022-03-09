# 安装
```bash
# 更新索引并安装依赖项
sudo apt update
sudo apt install software-properties-common apt-transport-https wget

# 导入GPG密钥并启用Webmin存储库
wget -q http://www.webmin.com/jcameron-key.asc -O- | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] http://download.webmin.com/download/repository sarge contrib"

# 更新索引并安装webmin，默认监听端口 10000，注意防火墙打开此端口
sudo apt update && sudo apt install webmin
```

# 关闭 SSl
```bash
# 设置 ssl=0
vi /etc/webmin/miniserv.conf
```

# 启动
```bash
service webmin start
systemctl start webmin
/etc/webmin/start
```