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



Как должно получиться ПРИМЕР:

```commandline
allow-hotplug enp0s3
iface enpos3 inet static
address 2.3.4.10
netmask 255.255.255.224

allow-hotplug enp0s8
iface enpos8 inet static
address 8.9.10.20
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

перезагружаем сеть `systemctl restart networking`, проверяем айпишники с помощью `ip a`, если всё сохранилось, то можно приступать к настройки ip на сервере Windows, потом проверить пинг и после успешной проверки дальнейшей установке служб пункт 2.

### **Второй этап** установка и настройка ADDS, DNS, DHCP, создание пользователей и ввод их в домен.

Полезные сылки: 

* https://softcomputers.org/blog/nastroika-windows-server-2019/
* https://habr.com/ru/companies/testo_lang/articles/525326/
* https://serveradmin.ru/nastroyka-kontrollerov-domena-v-raznyih-podsetyah/?ysclid=lwwlyw717y637249778 - соединение двух сервиров

### **Третий этап** установкa ЦС (installl_sentraSertifikat.pdf). 

### **Четвертый этап настройка ЦС (RDP.pdf или RDP(my).pdf)** 

### **Пятый этап MySQL** 
* https://lite.host/faq/hosting/komandnaya-stroka-mysql
* https://jeka.by/post/1003/rabotaem-s-mysql-cherez-komandnuyu-stroku/
* https://itreviewchannel.ru/rabota-s-mysql-cherez-komandnuyu-stroku/

### **Шестой этап OpenSSL** 
* https://blog.eldernode.com/configure-openssl-on-windows-server/
* https://applix.ru/articles/sozdanie-samopodpisannogo-ssl-sertifikata-na-windows-server/

### **Седьмой этап WireGuard** 
* https://introserv.com/ru/docs/wireguard-windows-setup/

`server`
```commandline
[Interface]
PrivateKey = 
ListenPort = 51820
Address = 12.0.0.1/24

[Peer]
PublicKey = <публичный ключ клиента>
AllowedIPs = 12.0.0.2/32
```


`client`
```commandline
[Interface]
PrivateKey = 
Address = 12.0.0.2/24

[Peer]
PublicKey =<публичный ключ сервера>
AllowedIPs = 5.4.1.0/0, ::/0
Endpoint = 1.2.3.1:51820
```




### !!!Кто хочет тупа копировать, то надо найти `.run` в терменале написать `bash` и путь к файлу `.run`  !!!


## Пример ниже для настройки vpn лучше смотерть в файле VPN.pdf

| Ф | Server                                                                                                                                                                                                                       | Client                                                                                                                                                                                                            |
|---|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Б | ``` [Interface] PrivateKey = <автоматом при заходе> ListenPort = 51820 Address = 10.0.0.1/24  [Peer] PublicKey = <публичный ключ клиента> AllowedIPs = 10.0.0.2/32, 192.168.1.0/24  ```                          | ``` [Interface] PrivateKey = <автоматом при заходе> Address = 10.0.0.2/24  [Peer] PublicKey = <публичный ключ сервер> AllowedIPs = 0.0.0.0/0, ::/0 Endpoint = 1.2.3.1:51820 ```                       |
| В | ``` [Interface] PrivateKey = <автоматом при заходе> ListenPort = 51820 Address = 192.168.100.1/24  [Peer] PublicKey = <публичный ключ клиента>  AllowedIPs = 192.168.100.2/32, 192.168.1.0/24  ```               | ``` [Interface] PrivateKey = <автоматом при заходе> Address = 192.168.100.2/24  [Peer] PublicKey = <публичный ключ сервер> AllowedIPs = 0.0.0.0/0, ::/0 Endpoint = 1.2.3.1:51820  ```                 |
| Г | ``` [Interface] PrivateKey = <автоматом при заходе> ListenPort = 51820 Address = 172.16.100.1/24  [Peer] PublicKey = <публичный ключ клиента>  AllowedIPs = 172.16.100.2/24, 172.16.1.0/24  ```                  | ``` [Interface] PrivateKey = <автоматом при заходе> Address = 172.16.100.2/24  [Peer] PublicKey = <публичный ключ сервер> AllowedIPs = 172.16.100.0/24 Endpoint = 1.2.3.1:51820  ```                  |
| З | ``` [Interface] PrivateKey = <автоматом при заходе> ListenPort = 51820 Address = 10.20.30.1/24  [Peer] PublicKey = <публичный ключ клиента> AllowedIPs = 10.20.30.2/32, 192.168.5.0/24  ```                      | ``` [Interface] PrivateKey = <автоматом при заходе> Address = 192.168.5.1/24  [Peer] PublicKey = <публичный ключ сервер> AllowedIPs = 10.20.30.1/32, 192.168.5.0/24 Endpoint = 1.2.3.1:51820  ```     |
| К | ``` [Interface] PrivateKey = <автоматом при заходе> ListenPort = 51820 Address = 172.16.81.1/24  [Peer] PublicKey =  AllowedIPs = 10.20.30.2/32  ```                                                                   | ``` [Interface] PrivateKey = <автоматом при заходе> Address = 10.20.30.2/32  [Peer] PublicKey = <публичный ключ сервер> AllowedIPs = 10.20.30.2/32 Endpoint = 1.2.3.1:51820  ```                      |
| П | ``` [Interface] PrivateKey = &amp;lt;автоматом при заходе&amp;gt; ListenPort = 51820 Address = 172.16.40.1/24  [Peer] PublicKey = &amp;lt;публичный ключ клиента&amp;gt; AllowedIPs = 172.16.40.2/32, 192.168.10.0/24  ```   | ``` [Interface] PrivateKey = <автоматом при заходе> Address = 192.168.10.1/24  [Peer] PublicKey = <публичный ключ сервер> AllowedIPs = 172.16.40.1/32, 192.168.10.0/24 Endpoint = 1.2.3.1:51820  ```  |
| Х | ``` [Interface] PrivateKey = GLzBeYvo8JoegJfER+CfPZD3g0YR2ejxteUZS25qS08= ListenPort = 51820 Address = 192.168.50.1/24  [Peer] PublicKey = <публичный ключ клиента> AllowedIPs = 192.168.50.2/32, 192.168.20.0/24  ``` | ``` [Interface] PrivateKey = <автоматом при заходе> Address = 192.168.20.1/24  [Peer] PublicKey = <публичный ключ сервер> AllowedIPs = 192.168.50.1/32, 192.168.20.0/24 Endpoint = 1.2.3.1:51820  ``` |