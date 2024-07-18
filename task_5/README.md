1. Создайте отдельную директорию(например /tmp/netology/docker/task5) и 2 файла внутри него. "compose.yaml" с содержимым:
```
version: "3"
services:
  portainer:
    image: portainer/portainer-ce:latest
    network_mode: host
    ports:
      - "9000:9000"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```

"docker-compose.yaml" с содержимым:

```version: "3"
services:
  registry:
    image: registry:2
    network_mode: host
    ports:
    - "5000:5000"
```

И выполните команду "docker compose up -d". Какой из файлов был запущен и почему? (подсказка: https://docs.docker.com/compose/compose-application-model/#the-compose-file )

2. Отредактируйте файл compose.yaml так, чтобы были запущенны оба файла. (подсказка: https://docs.docker.com/compose/compose-file/14-include/)

3. Выполните в консоли вашей хостовой ОС необходимые команды чтобы залить образ custom-nginx как custom-nginx:latest в запущенное вами, локальное registry. Дополнительная документация: https://distribution.github.io/distribution/about/deploying/

4. Откройте страницу "https://127.0.0.1:9000" и произведите начальную настройку portainer.(логин и пароль адмнистратора)

5. Откройте страницу "http://127.0.0.1:9000/#!/home", выберите ваше local окружение. Перейдите на вкладку "stacks" и в "web editor" задеплойте следующий компоуз:

```
version: '3'

services:
  nginx:
    image: 127.0.0.1:5000/custom-nginx
    ports:
      - "9090:80"
```

6. Перейдите на страницу "http://127.0.0.1:9000/#!/2/docker/containers", выберите контейнер с nginx и нажмите на кнопку "inspect". В представлении <> Tree разверните поле "Config" и сделайте скриншот от поля "AppArmorProfile" до "Driver".

7. Удалите любой из манифестов компоуза(например compose.yaml). Выполните команду "docker compose up -d". Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите compose-проект ОДНОЙ(обязательно!!) командой.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод, файл compose.yaml , скриншот portainer c задеплоенным компоузом.




1. Путь по умолчанию для файла Compose — compose.yaml(предпочтительно) или compose.yml, который находится в рабочем каталоге. Compose также поддерживает docker-compose.yamlи docker-compose.ymlдля обратной совместимости с более ранними версиями. Если существуют оба файла, Compose предпочитает канонический compose.yaml.

![изображение](https://github.com/stepynin-georgy/hw_netology_docker/blob/main/task_5/Screenshot_14.png)

2. 
```
[root@hw-netology task_5]# cat compose.yaml
include:
  - docker-compose.yaml
services:
  portainer:
    image: portainer/portainer-ce:latest
    network_mode: host
    ports:
      - "9000:9000"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
[root@hw-netology task_5]# cat docker-compose.yaml
services:
  registry:
    image: registry:2
    network_mode: host
    ports:
    - "5000:5000"
```

3. ![изображение](https://github.com/stepynin-georgy/hw_netology_docker/blob/main/task_5/Screenshot_15.png)
4. ![изображение](https://github.com/stepynin-georgy/hw_netology_docker/blob/main/task_5/Screenshot_16.png)
5. ![изображение](https://github.com/stepynin-georgy/hw_netology_docker/blob/main/task_5/Screenshot_18.png)
![изображение](https://github.com/stepynin-georgy/hw_netology_docker/blob/main/task_5/Screenshot_18.png)
6. ![изображение](https://github.com/stepynin-georgy/hw_netology_docker/blob/main/task_5/Screenshot_19.png)
7. При удалении одного из файлов, высвечивается предупреждение о ненужных контейнерах , предлагается их удалить командой --remove-orphans

![изображение](https://github.com/stepynin-georgy/hw_netology_docker/blob/main/task_5/Screenshot_20.png)