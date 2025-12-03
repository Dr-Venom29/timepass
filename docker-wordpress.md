 WordPress + MySQL (Docker Compose) — Steps + Commands Only
Step 1: Create project folder
mkdir wordpress-mysql
cd wordpress-mysql

Step 2: Create docker-compose.yml

Create file:

docker-compose.yml


Paste:

version: '3.8'

services:
  db:
    image: mysql:5.7
    container_name: wp-mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpass
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wpuser
      MYSQL_PASSWORD: wppass
    volumes:
      - mysql_data:/var/lib/mysql

  wordpress:
    image: wordpress:latest
    container_name: wp-app
    restart: always
    ports:
      - "5050:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wpuser
      WORDPRESS_DB_PASSWORD: wppass
      WORDPRESS_DB_NAME: wordpress
    depends_on:
      - db

volumes:
  mysql_data:

Step 3: Start containers
docker compose up -d

Step 4: Check running containers
docker ps

Step 5: View logs (optional)

MySQL:

docker compose logs db


WordPress:

docker compose logs wordpress

Step 6: Open WordPress
http://localhost:5050


Complete setup → Site title, username, password → Install WordPress.

Step 7: Login to admin
http://localhost:5050/wp-admin

Step 8: Stop containers
docker compose down

Step 9: Stop + delete volume (delete DB)
docker compose down -v
