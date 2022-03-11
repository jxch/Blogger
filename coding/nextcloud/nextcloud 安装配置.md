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
      - MARIADB_DATABASE=nextcloud
      - MARIADB_USER=nextcloud
      - MARIADB_PASSWORD=nextcloud
  nextcloud:
    image: nextcloud
    restart: unless-stopped
    ports:
      - 8081:80
    depends_on:
      - mariadb
    environment:
      - MYSQL_HOST=mariadb
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
      - MYSQL_PASSWORD=nextcloud
    volumes:
      - ./nextcloud/var:/var/www/html
```