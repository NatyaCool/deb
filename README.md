### **Первый этап** настройка Deb1,2,3:
    
```commandline
    su
    nano /etc/network/interfaces
```

    
В файле `nano /etc/network/interfaces` надо написать ip адреса по заданию 

(<?> нужно изменить на IP адреса):

```commandline
    allow-hotplug enp0s3
    iface enpos3 inet static
    address <ip> //здесь надо ставить ip-адрес
    netmask <mask> //здесь надо ставить маску подсети

    allow-hotplug enp0s3
    iface enpos8 inet static
    address <ip> //здесь надо ставить ip-адрес
    netmask <mask> //здесь надо ставить маску подсети

    up ip route add <ip> via 8.9.10._/__
    up ip route add <mask> via 8.9.10._/__
```



Как должно получиться:

```commandline
allow-hotplug enp0s3
iface enpos3 inet static
address 2.3.4.10
netmask 255.255.255.224

allow-hotplug enp0s8
iface enpos8 inet staticaddress 8.9.10.20
netmask 255.255.255.0

up ip route add 1.2.3.0/28 via 8.9.10.10
up ip route add 5.6.7.0/26 via 8.9.10.30
```

затем сохроняем ctrl+o, и выходим ctrl+x из режима редактирования.

`systemctl restart networking`\
`systemctl status networking`


Затем вбить команду ниже и убрать в оцне редактирование знак `#` с `net.ipv4.ip_forward=1`

```
nano /etc/sysctl.conf
```

Затем прописывается маршрут (IP меняются как в задании):

```commandline
ip route add 5.6.7.0/26 via 8.9.10.30
ip route add 2.3.4.0/27 via 8.9.10.20
```


перезагружаем сеть `systemctl restart networking`, проверяем айпишники с помощью `ip a`, если всё сохранилось, то можно приступать к настройки ip на сервере Windows, потом проверить пинг и после успешной проверки дальнейшей установке служб пункт 2.

### **Второй этап** установка и настройка ADDS, DNS, DHCP, создание пользователей и ввод их в домен.

Полезные сылки: 

* https://softcomputers.org/blog/nastroika-windows-server-2019/
* https://habr.com/ru/companies/testo_lang/articles/525326/
### **Третий этап** установка ЦС. 

Полезные сылки: https://abuzov.com/active-directory-certificate-services/

https://dev.rutoken.ru/pages/viewpage.action?pageId=57148969

### **Четвертый этап установка ЦС** 

### **Пятый этап OpenSSL** 

### **Шестой этап MySQL** 

### **Седьмой этап WireGuard** 
