## Работа с Docker

### 1 - Устанавливаем Докер

Обновим списки пакетов:
__
    sudo apt update
Установим пакеты, которые позволят использовать репозиторий по HTTPS:
__
    sudo apt install apt-transport-https ca-certificates curl software-properties-common
Добавим официальный GPG-ключ Docker:
__
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

Добавим репозиторий Docker к списку источников пакетов:
__
    echo "deb [signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

Обновим список пакетов, чтобы включить информацию о пакетах Docker из добавленного репозитория:
__
    sudo apt update
Установите Docker:
__
    sudo apt install docker-ce
Добавьте вашего пользователя в группу docker, чтобы избежать использования sudo для запуска Docker команд:
__
    sudo usermod -aG docker $USER
Перезагрузите систему или запустите следующую команду, чтобы применить изменения в текущем сеансе:
__
    newgrp docker
Теперь вы должны быть готовы использовать Docker через терминал. Вы можете проверить его работу, выполнив команду:
__

    docker --version

![Alt text](3.jpg)

Докер установили....

### 2 - Тестируем.

Запустим контейнер из образа Ubuntu и войдем в него:
__

    docker run docker/whalesay cowsay Hello, Docker!
![Alt text](4.jpg)

    docker run docker/whalesay cowsay -f elephant "Hello, Docker!"
![Alt text](4.1.jpg) !


### 3 - Запускаем контейнер из образа Ubuntu и входим в него:

docker run -it -h GB --name gb-test ubuntu:22.10
![Alt text](5.jpg)
Далее создадим новую директорию в корневом каталоге:

    mkdir /example

Теперь создадим файл "passwords.txt" и добавим в него данные:
![Alt text](6.jpg)

![Alt text](8.jpg)

### 4 - Остановка контейнера и повторный запуск его.
Выполним последовательно команды:


    docker stop gb-test

    docker start gb-test

    docker exec -it gb-test bash

    cat /example/passwords.txt

    exit

![Alt text](9.jpg)

Так как мы не пересоздавали контейнер, все наши данные будут сохранены.

### 5 - Удаление контейнера и создадание его снова. Используются те же команды:

    docker stop gb-test

    docker rm gb-test

    docker run -it -h GB --name gb-test ubuntu:22.10

    ls -l

    exit

    docker stop gb-test

    docker rm gb-test

![Alt text](10-1.jpg)


Из-за того, что контейнер удален, все наши даннные пропали.

### 6 - Создаение новой директории и присоединение ее к контейнеру.

    mkdir /test/folder
    docker run -it -h GB --name gb-test -v /test/folder:/otherway ubuntu:22.10

![Alt text](11.jpg)

### 7 - Добавление данных в присоединенную директорию:

    echo "$HOSTNAME" >> /otherway/test.txt

    ls
![Alt text](13.jpg)


### 8 - Удаление контейнера и создание его снова, монтирование в директорию, ичпользуя команды:

    docker stop gb-test

    docker rm gb-test

    docker run -it -h GB --name gb-test -v /test/folder:/otherway ubuntu:22.10

    cat /otherway/test.txt

    exit

    docker stop gb-test

    docker rm gb-test

![Alt text](14.jpg)
### 9 - Хранение данных в контейнерах Docker.

Создадим две папки. Заметим, что содержимое этих папок разное. Эти папки нужны нам для будущего монтирования. Контейнер создадим  из образа ubuntu:22.10.

    mkdir ~/docker-mount-example

    echo "This is the host test.txt file" > ~/docker-mount-example/test.txt

    echo "This is the root test.txt file" > ~/test.txt

    docker run -it -h GB --name gb-test -v ~/docker-mount-example:/container-mount -v ~/test.txt:/container-mount/test.txt ubuntu:22.10

![Alt text](16.jpg)
Тем самым содержимое файла /container-mount/test.txt было перезаписано вторым монтированием из файла ~/test.txt
