1. Воспользуйтесь docker help или google, чтобы узнать как подключиться к стандартному потоку ввода/вывода/ошибок контейнера "custom-nginx-t2".
1. Подключитесь к контейнеру и нажмите комбинацию Ctrl-C.
1. Выполните docker ps -a и объясните своими словами почему контейнер остановился.
1. Перезапустите контейнер
1. Зайдите в интерактивный терминал контейнера "custom-nginx-t2" с оболочкой bash.
1. Установите любимый текстовый редактор(vim, nano итд) с помощью apt-get.
1. Отредактируйте файл "/etc/nginx/conf.d/default.conf", заменив порт "listen 80" на "listen 81".
1. Запомните(!) и выполните команду nginx -s reload, а затем внутри контейнера curl http://127.0.0.1:80 ; curl http://127.0.0.1:81.
1. Выйдите из контейнера, набрав в консоли exit или Ctrl-D.
1. Проверьте вывод команд: ss -tlpn | grep 127.0.0.1:8080 , docker port custom-nginx-t2, curl http://127.0.0.1:8080. Кратко объясните суть возникшей проблемы.
1. * Это дополнительное, необязательное задание. Попробуйте самостоятельно исправить конфигурацию контейнера, используя доступные источники в интернете. Не изменяйте конфигурацию nginx и не удаляйте контейнер. 1.1.1.1.Останавливать контейнер можно. пример источника
1. Удалите запущенный контейнер "custom-nginx-t2", не останавливая его.(воспользуйтесь --help или google)
В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

# Решение: 
1. Контейнер завершил свою работу, т.к. на вход контейнера был послан сигнал cntl-c = "signal 2 (SIGINT)" . Контейнер отработал сигнал и завершил работу.

```
[root@hw-netology hw]# docker ps
CONTAINER ID   IMAGE                COMMAND                  CREATED        STATUS         PORTS                                   NAMES
c31d64d94172   custom_nginx:1.0.0   "/docker-entrypoint.…"   20 hours ago   Up 4 seconds   0.0.0.0:8080->80/tcp, :::8080->80/tcp   custom-nginx-t2
[root@hw-netology hw]# docker attach custom-nginx-t2
^C2024/07/16 06:38:44 [notice] 1#1: signal 2 (SIGINT) received, exiting
2024/07/16 06:38:44 [notice] 24#24: exiting
2024/07/16 06:38:44 [notice] 24#24: exit
2024/07/16 06:38:44 [notice] 25#25: exiting
2024/07/16 06:38:44 [notice] 25#25: exit
2024/07/16 06:38:44 [notice] 1#1: signal 17 (SIGCHLD) received from 24
2024/07/16 06:38:44 [notice] 1#1: worker process 24 exited with code 0
2024/07/16 06:38:44 [notice] 1#1: signal 29 (SIGIO) received
2024/07/16 06:38:44 [notice] 1#1: signal 17 (SIGCHLD) received from 25
2024/07/16 06:38:44 [notice] 1#1: worker process 25 exited with code 0
2024/07/16 06:38:44 [notice] 1#1: exit
[root@hw-netology hw]# docker ps -a
CONTAINER ID   IMAGE                COMMAND                  CREATED        STATUS                     PORTS     NAMES
c31d64d94172   custom_nginx:1.0.0   "/docker-entrypoint.…"   20 hours ago   Exited (0) 7 seconds ago             custom-nginx-t2
```

1. `docker start custom-nginx-t2`
1. 
```
[root@hw-netology hw]# docker exec -ti custom-nginx-t2 bash
root@c31d64d94172:/# apt-get update
root@c31d64d94172:/# apt-get install vim
```

После замены, внутри контейнера:
```
root@c31d64d94172:/# curl http://127.0.0.1:80 ; curl http://127.0.0.1:81
curl: (7) Failed to connect to 127.0.0.1 port 80: Connection refused
<html>
        <head>
                Hey, Netology
        </head>
        <body>
                <h1>I will be DevOps Engineer!</h1>
        </body>
</html>
```

На хостовой ОС:
```
[root@hw-netology hw]# ss -tlpn | grep 127.0.0.1:8080
[root@hw-netology hw]# docker ps -a
CONTAINER ID   IMAGE                COMMAND                  CREATED        STATUS         PORTS                                   NAMES
c31d64d94172   custom_nginx:1.0.0   "/docker-entrypoint.…"   20 hours ago   Up 5 minutes   0.0.0.0:8080->80/tcp, :::8080->80/tcp   custom-nginx-t2
[root@hw-netology hw]# docker port custom-nginx-t2
80/tcp -> 0.0.0.0:8080
80/tcp -> [::]:8080
[root@hw-netology hw]# curl http://127.0.0.1:8080
curl: (56) Recv failure: Connection reset by peer
[root@hw-netology hw]#
```

Внешний порт 8080 пробрасывает на 80 порт внутри контейнера. Т.к. nginx'ом прослушывается 81 порт, то и 127.0.0.1:8080 не будет работать. 

1. `[root@hw-netology hw]# docker rm --force custom-nginx-t2`
