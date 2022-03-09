```bash
# 配置用户名密码
git config --global user.name username
# 使用 token
git config --global user.password token
git config --global user.email "email"

# 输入的用户名密码将被记住，不用每次都输入了
git config --global credential.helper store
```