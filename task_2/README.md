1. Запустите ваш образ custom-nginx:1.0.0 командой docker run в соответвии с требованиями:
* имя контейнера "ФИО-custom-nginx-t2"
* контейнер работает в фоне
* контейнер опубликован на порту хост системы 127.0.0.1:8080
2. Переименуйте контейнер в "custom-nginx-t2"
3. Выполните команду date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8080  ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html
4. Убедитесь с помощью curl или веб браузера, что индекс-страница доступна.
В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

![изображение](https://github.com/stepynin-georgy/hw_netology_docker/blob/main/task_2/Screenshot_2.png)
![изображение](https://github.com/stepynin-georgy/hw_netology_docker/blob/main/task_2/Screenshot_3.png)