__Задание:__ *запустить контейнер с ubuntu, используя механизм LXC, ограничить контейнер 256 Мб ОЗУ и проверить, что ограничение работает*

1. Установка необходимых пакетов, создание lxc контейнера


apt-get install lxc debootstrap bridge-utils lxc-templates
![Alt text](1.jpg)
apt-get install lxd-installer
![Alt text](2.jpg)
lxd init
![Alt text](3-1.jpg)
Проверяем:
lxc storage list
![Alt text](4.jpg)
lxc-create -n test01 -t ubuntu -f /usr/share/doc/lxc/example/
lxc-veth.conf
![Alt text](5.jpg)

2. Открываем файл конфигурации /var/lib/lxc/test123/config
![Alt text](6.jpg)

3. Далее вставляем в файл конфигурации следующую строку:
lxc.cgroup2.memory.max = 256M
![Alt text](7.jpg)

    После этого нужно сохраниться и выхйти. Теперь можно запустить контейнер и зайти в него.

    lxc-start -n test01

    lxc-attach -n test01 
    ![Alt text](8.jpg)

    Проверяем память, введя команду: free -m и получаем следующий результат:
    ![Alt text](9.jpg)

    


