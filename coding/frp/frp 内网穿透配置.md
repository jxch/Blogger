# 服务端

## `frps.ini`
```ini
[common]
bind_port=7000
token=jiang155.
dashboard_port=7500
dashboard_user=admin
dashboard_pwd=jiang155.
vhost_http_port=8081
vhost_https_port=9091

tcp_mux=true
max_pool_count=10

[ssh]
listen_port=2222
```

## `docker-compose.yml`
```yml
  frps:
    image: snowdreamtech/frps
    restart: always
    ports:
      - 7000:7000
      - 7500:7500
      - 2222:2222
      - 8081-8089:8081-8089
      - 9091-9099:9091-9099
    volumes:
      - /home/opc/service/frps/frps.ini:/etc/frp/frps.ini
```

# 客户端

## `frpc.ini`
```ini
[common]
server_addr=jiangxicheng.xyz
server_port=7000
token=jiang155.

[ssh]
type=tcp
local_ip=127.0.0.1
local_port=22
remote_port=2222
use_encryption=true
use_compression=true
```

## `docker-compose.yml`
```yml
  frpc:
    image: snowdreamtech/frpc
    restart: always
    volumes:
      - ./frpc/frpc.ini:/etc/frp/frpc.ini
    network_mode: host
```

# 连接 web 服务
## SSH
1. 将内网SSH公钥安装到公网服务器中：追加到此文件中 `/root/.ssh/authorized_keys`
2. 然后用私钥登录：`ssh 内网user:pwd@公网ip ssh-frp端口(2222)`