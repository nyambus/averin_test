### Добавление клиентов
---
На клиенте:
```
wget https://repo.zabbix.com/zabbix/7.2/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.2+ubuntu22.04_all.deb
dpkg -i zabbix-release_latest_7.2+ubuntu22.04_all.deb
apt update 
apt install zabbix-agent2
```
`nano /etc/zabbix/zabbix-agent2.conf`:
```
Server=<адрес сервера>
ServerActive=<адрес сервера>
Hostname=example
```

В zabbix:

  `Monitoring - Hosts - Create host`

Меню `New host`: 
    
    Host name: example
  
    Templates: Linux by Zabbix agent
  
    Host groups: Virtual machines/Zabbix servers
  
    Interfaces: Add - Agent - IP address
  
    Add


### Билеты
---
2, 6. Проанализируйте работу нескольких устройств и постройте графики их показателей с помощью Zabbix. 

  а) Добавляем клиентов.
  
  б) Графики:

       Dashboards - Actions (три палочки) - Create New
     
         Name: Example
     
       Меню Add widget
     
         Type: Graph
     
         Name: Example
     
         Refresh interval: 10 seconds
     
         Data set - host patterns - Select - Virtual machines - client1; item patterns  - Select - CPU utilization + Memory utilization

5. Проанализируйте работу сети с использованием протокола SNMP.

В windows 10:

    Settings - Apps - Optional features - Add a feature - SNMP

7. Постройте топологию сети с помощью Zabbix.

  а) Добавляем клиентов. 

  б) Карты:
  
    Monitoring - Maps - Create map
    
      Name: Example
      
      Icon highlight + 
      
      Mark elements on trigger status change +
      
      Number of problems and expand most critical one
      
      Add
      
    Example - Edit map 
    
    Map element - Add 

      Type: Host

      Label: example

      Host: client/server

      Icons: zabbix-server/server

    Link - Add
    
9. Проанализируйте работу сервера и выведите графики загруженности за разный период времени.

  а) Добавляем клиентов.
  
  б) Графики:

       Dashboards - Actions (три палочки) - Create New
     
         Name: Example
     
       Меню Add widget
     
         Type: Graph
     
         Name: Example
     
         Refresh interval: 10 seconds
     
         Data set - host patterns - Select - Virtual machines - client1; item patterns  - Select - CPU utilization + Memory utilization

