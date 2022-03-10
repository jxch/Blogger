# 安装 `docker`
```bash
curl -fsSL https://get.docker.com | bash -s docker
```

# 修改 `daemon.json`
```json
{
  # 修改镜像地址
  "registry-mirrors":["https://docker.mirrors.ustc.edu.cn/"]
  # 修改docker默认存储位置
  "graph": "/media/disk/docker"
}
```

```bash
# 修改之后重启服务
systemctl daemon-reload
systemctl restart docker
# 查看配置是否生效
docker info
```
