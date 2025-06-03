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
