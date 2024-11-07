# BR-SRV
```
"TYPE=eth
DISABLED=no
NM_CONTROLLED=no
BOOTPROTO=static
CONFIG_IPv4=yes" > /etc/net/ifaces/ens192/options
```

```
192.168.1.2/26 > /etc/net/ifaces/ens192/ipv4address
```

```
default via 192.168.1.1 > /etc/net/ifaces/ens192/ipv4route
```

```
nameserver 192.168.0.2 > /etc/net/ifaces/ens192/resolv.conf
nameserver 192.168.0.2 > /etc/resolv.conf
```

```
поставить # в строчке nameserver > /etc/resolvconf.conf
```

```
systemctl restart network
```
Делаем на всякий случай бэкап конфига:

```
cd /etc/openssh
cp -a sshd_config sshd_config.bak
```

Редактируем файл конфигурации SSH сервера

```
nano sshd_config
```

Изменяем следующие параметры. Не забываем их раскоментировать. Если какой-то параметр не находиться, то просто добавьте его сами

```
Port 2024
MaxAuthTries 3
Banner /etc/openssh/banner
AllowUsers sshuser
```

Чтобы зайти через PUTTY нужно ввести команды на обоих машинах Linux

```
systemctl start serial-getty@ttyS0.service
systemctl enable serial-getty@ttyS0.service
systemctl status serial-getty@ttyS0.service
```

Создаем файл с баннером

```
nano /etc/openssh/banner
```

Вставляем в него следующие содержимое

```
******************************************************
*                     WARNING!                       *
*                                                    *
*           Access only authorized users!            *
*                                                    *
*  If you not authorized user, close this session!   *
*                                                    *
******************************************************
```
```
useradd -m -u 1010 sshuser
passwd sshuser
```

```
nano /etc/sudoers.d/sshuser
```

```
sshuser ALL=(ALL) NOPASSWD:ALL
```

```
su - sshuser
sudo whoami
```

Перезагружаем `SSH`

```
systemctl restart sshd
```
## Проверка

```
На машине BR-SRV вводим: ssh sshuser@hq-srv.au-team.irpo -p 2024
```
Проверяем какой часовой пояс установлен

```
timedatectl status
```
Если отличается, то устанавливаем

```
timedatectl set-timezone Asia/Yekaterinburg
```
