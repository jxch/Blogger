```bash
# 建立密钥对（这里是在root用户下）
ssh-keygen

# 安装公钥
cd ~/.ssh
cat id_rsa.pub >> authorized_keys
# 确保权限正确
chmod 600 authorized_keys
chmod 700 ~/.ssh

# 配置文件
vi /etc/ssh/sshd_config
RSAAuthentication yes
PubkeyAuthentication yes
PermitRootLogin yes
# 可以关闭密码登录
PasswordAuthentication no

# 重启SSH服务
service sshd restart
```