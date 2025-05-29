1. IP-адреса

    Проверить настройки сети:

        ovs-vsctl show

        ip a

        ip r

        iptables -t nat -L

    Для различных устройств:

        ideco: сетевые интерфейсы

        eco: sh ip int br

        linrtr: sh interface brief

2. Маршрутизация

    Проверить маршруты:

        ip r

        Для ideco:

            Маршрутизация -> локальные сети

        Для eco:

            sh ip ospf neighbour, sh ip route ospf, sh run

        Для linrtr:

            sh ip ospf neighbour, sh ip route ospf, sh run

        Для zoo-rtr:

            ip r

            Проверьте /etc/network/interfaces (секция GRE)

    Проверка трафика по определенным маршрутам.

3. VPN (GRE over IPSec)

    debian GRE:

        cat /etc/modules

        ip a

    ideco GRE:

        Сетевые интерфейсы -> туннельные

    debian IPSec:

        ipsec status/ipsec statusall

        systemctl status strongswan-starter

        ping 12.8.0.2

        Проверьте конфигурации:

            nano /etc/ipsec.conf

            cat /etc/ipsec.secrets

    ideco IPSec:

        Сервисы -> ipsec -> Входящие -> "Установлено"

4. VLAN

    Проверить настройки VLAN:

        sw-ens: ovs-vsctl show

        ideco: Сетевые интерфейсы

5. Бонды

    Пропустить.

6. Мониторинг

    Zabbix:

        Добавить серверы из локальных сетей, использовать TLS.

        На вкладке "Шифрование" проверить зеленую отметку.

        Создать дэшборд с основными показателями хостов.

        Добавить карту сети с триггерами на доступность Zabbix-агентов.

        Добавить список проблем.

    Rsyslog:

        Ранее в инфраструктуре не было сервера логов.

        Необходимо было как можно быстрее настроить пересылку логов с сервера zoo.

        Для других серверов настройка rsyslog не была завершена.

        На srv1:

            ls -la /opt/logs

            cat /opt/logs/*.log

        Проверка статуса rsyslog:

            systemctl status rsyslog

            nano /etc/rsyslog.d/00-common.conf

            ss -tunlp | grep 514

        На клиенте:

            systemctl status rsyslog

            cat /etc/rsyslog.conf

7. Ideco файрвол

    Показать правила трафика:

        Переход в "Файрвол" -> "Forward".

    Показать отключенное правило для запрета всего.

    Показать настройки SSH и доступа к вебу из внешней сети:

        "Управление сервером" -> "Администраторы" -> "Настройки SSH".

8. Альт Домен (SAMBA)

    samba-tool domain info 127.0.0.1

    klist

    admc

9. Автозагрузка Firefox с Zabbix

    Выполнить перезагрузку:

        reboot

    Настроить автозагрузку:

        nano /.config/autostart/firefox-...

10. Подключение по SSH

    Проверить подключение к различным серверам:

        ssh srv1

        ssh dc

        ssh 192.168.10.10

11. Шара

    Завершить сессию:

        kdestroy

    Инициализировать сессию:

        kinit administrator

    Открыть шару в проводнике:

        smb://srv1.dtc.kolobki.com

    Выйти из шара:

        ПКМ -> Отмонтировать

    Завершить сессию:

        kdestroy

    Инициализировать сессию под другим пользователем:

        kinit fox

    Войти в шару:

        Ввод пароля.

12. Сертификаты

    Создание сертификатов на srv1:

        В директории /ca создать сертификат через скрипт:

            ./openssl-enroll.sh domain.com domaindir

    Проверка доступа с pc-ens:

        curl https://srv1.dtc.kolobki.com:8000

    Доступ через веб-интерфейс:

        http://srv1.dtc.kolobki.com

13. Лазейка

    Доступ к веб-интерфейсу через:

        192.168.100.1:8080 user:P@ssw0rd

    Веб-интерфейс с правами администратора:

        Разрешены только ICMP в сторону PC.

14. RAID

    Проверка RAID-массива:

        cat /etc/fstab

        lsblk -f

        mdadm --detail /dev/md127

    Видео доступно на:

        pc-ens /home/kolobki/
