DEMKA

Первый этап настройка Deb1,2,3:

su

nano /etc/network/interfaces

в файле nano /etc/network/interfaces надо написать(то сто в <?> может отличатся):

allow-hotplug enp0s3

iface enpos3 inet static

address <ip>

netmask <mas>

    up ip route add 5.6.7.0/26 via 8.9.10.1
    
    up ip route add 2.3.4.0/27 via 8.9.10.3
    
затем сохроняем ctrl+o, и выходим ctrl+x

Перезагрузка затем ниже

nano /etc/sysctl.conf (убрать # с net.ipv4.ip_forward=1)

затем прописывается маршрут:

ip route add 5.6.7.0/26 via 8.9.10.30

ip route add 2.3.4.0/27 via 8.9.10.20

перезагружаем, проверяем айпишники с помощью ip a, если всё сохронилось то можно приступать к настройки ip на сервере, потом проверить пинг и после успешной проверки дальнейшей установке служб пункт 2.
