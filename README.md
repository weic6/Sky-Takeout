## Front-end

1. download nginx:`brew install nginx`
2. download front-end code: [sky-take-out-frontend](https://github.com/weitianpaxi/sky_take_out/blob/main/sky_take_out%E7%AE%A1%E7%90%86%E7%AB%AF%E5%89%8D%E7%AB%AF%E8%BF%90%E8%A1%8C%E7%8E%AF%E5%A2%83.zip), and unzip file and get `前端运行环境`
3. copy `sky` folder, in front-end code, into nginx folder `/opt/homebrew/var/www`. (`brew info nginx` -> `Docroot is: /opt/homebrew/var/www`)
4. copy nginx.conf from `/前端运行环境/nginx-1.20.2/conf`, and replace the original nginx.conf in `/opt/homebrew/etc/nginx` (`brew info nginx`, and look for address like `/opt/homebrew/etc/nginx/nginx.conf`)
5. run `brew services start nginx`, and visit `localhost` in browser. To stop nginx, run `brew services stop nginx`
6. Done

Reference:

1. https://blog.csdn.net/m0_74570541/article/details/133352659
2. https://blog.csdn.net/qq_45828494/article/details/131982807
3. https://github.com/weitianpaxi/sky_take_out/tree/main

## Database

The schema is provided in `sky.sql`. All actual data lives in a temporary Docker volume, so you can delete it at will.

Follow the steps below to bring up a database for local development, seed it with demo data, and tear it down again.

```python
# start all services (includes service `db`) defined in the docker-compose.yml
docker compose -f docker-compose.yaml up -d

# view the logs of the db service defined in your docker-compose.yml
docker compose logs -f db

# inside the db container, creates the database and loads all data.
docker compose exec -T db \
  mysql -u root -p123456 < db/sky.sql # no space between -p and secret!
#  • `docker compose exec db …` → “find the container for service ‘db’ (i.e. sky-mysql) and run … inside it.”
#  • `mysql -u root -p123456` → launch the MySQL CLI as root, with password 123456.
#  • `< db/sky.sql` → on your Mac, read the file `db/sky.sql` and send it into that CLI’s stdin.
#  Because sky.sql’s first lines say “CREATE DATABASE IF NOT EXISTS sky_take_out; USE sky_take_out; …”,
#  MySQL will create the database and load all tables + seed data in one shot.

# use database `sky_take_out`
docker compose exec db \
  mysql -u root -p123456 sky_take_out
#  You’ll see a prompt like:
#
#       mysql> SHOW TABLES;
#       +-------------------+
#       | Tables_in_sky_take_out |
#       +-------------------+
#       | address_book      |
#       | category          |
#       | dish              |
#       | …                 |
#       +-------------------+

# remove/create container
docker compose -f docker-compose.yaml down
docker compose -f docker-compose.yaml up -d

# pause/start container
docker compose -f docker-compose.yaml stop
docker compose -f docker-compose.yaml start
```
