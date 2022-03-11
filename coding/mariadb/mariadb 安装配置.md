# `docker-compose.yml`
```yml
  mariadb:
    image: mariadb
    restart: unless-stopped
    ports:
      - 3306:3306
    volumes:
      - ./mariadb/data:/var/lib/mysql
    environment:
      - MARIADB_ROOT_PASSWORD=jiang155.
```

# 基本操作
## 赋权
```sql
CREATE database wordpress;
CREATE USER 'wordpress'@'%' IDENTIFIED BY 'wordpress';
grant all privileges on wordpress.* to wordpress@'%' identified by 'wordpress';
grant all privileges on wordpress.* to wordpress@'127.0.0.1' identified by 'wordpress';
flush privileges;
```