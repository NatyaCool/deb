dugati

### **Первый этап** настройка Deb1,2,3:
    
    su

    nano /etc/network/interfaces
в файле nano /etc/network/interfaces надо написать(то что в <?> может отличатся):

    allow-hotplug enp0s3

    iface enpos3 inet static
    
    address <ip> //здесь надо ставить ip-адрес 

    netmask <mask> //здесь надо ставить маску подсети

    auto enp0s8

    iface enpos8 inet static
    
    address <ip> //здесь надо ставить ip-адрес 

    netmask <mask> //здесь надо ставить маску подсети

    up ip route add <ip> via 8.9.10._
    
    up ip route add <mask> via 8.9.10._ 

    ## Последний актет меняется 


    
затем сохроняем ctrl+o, и выходим ctrl+x

    systemctl restart networking

    systemctl status networking

Затем перейти

    nano /etc/sysctl.conf (убрать # с net.ipv4.ip_forward=1)

затем прописывается маршрут:

    ip route add 5.6.7.0/26 via 8.9.10.30

    ip route add 2.3.4.0/27 via 8.9.10.20

перезагружаем, проверяем айпишники с помощью ip a, если всё сохронилось то можно приступать к настройки ip на сервере, потом проверить пинг и после успешной проверки дальнейшей установке служб пункт 2.

### **Второй этап** установка и настройка ADDS, DNS, DHCP, создание пользователей и ввод их в домен.

Полезные сылки: https://softcomputers.org/blog/nastroika-windows-server-2019/

https://habr.com/ru/companies/testo_lang/articles/525326/

### **Третий этап** установка ЦС. 

Полезные сылки: https://abuzov.com/active-directory-certificate-services/

https://dev.rutoken.ru/pages/viewpage.action?pageId=57148969

### **Четвертый этап установка ЦС** 

### **Пятый этап OpenSSL** 

### **Шестой этап MySQL** 

### **Седьмой этап WireGuard** 
