### Базовая настройка:
---
	1.1 Поставить ip на виртуальный интерфейс:
```
interface int0
	ip address 192.168.1.2/24
```

	1.2 Создать на физическом порту service-instance, указать нетегированный трафик и подключить виртуальный интерфейс (вариант без vlan):
```
port ge0
	service-instance WAN
		encapsulation untagged
		connect ip interface int0
```
	Если нужны vlan:
```
port ge0
	service-instance WAN
		encapsulation dot1q
		rewrite pop ?
```

3. Установить маршрут по умолчанию:
```
ip route 0.0.0.0/0 192.168.1.1
```

### NAT
---
1. Расставляем inside/outside на интерфейсах:
```
interface int0 
	ip nat outside
interface int1
	ip nat inside
```

2. Создаем пул source-адресов, которые будут натиться:
```
ip nat pool NAME 172.16.1.1-172.16.1.255
```

3. Включаем динамическую трансляцию пула через внешний адрес:
```
ip nat source dynamic inside-to-outside pool NAME overload 192.168.1.2
```


### Создание пользователя + админские права
---
1. Создание пользователя с правами администратора:
```
username sshuser
	password P@ssw0rd
	role admin
```

2. Для просмотра списка пользователей и их прав можно использовать:
```
show users connected
```

### Разрешение SSH + профили безопасности и VRF
---
Все работает через vrf и профили безопасности, в дефолтном профиле безопасности запрещен доступ по ssh.

1. Создаем свой профиль, разрешая что угодно (в нашем случае доступ по ssh):
```
security-profile 1
	rule number permit/deny protocol source destination [eq port number]
```
Есть также вариант использовать уже созданный профиль `none`.

2. Если есть необходимость создать отдельную vrf :
```
ip vrf vrf_name
```

3. Применение профиля к vrf:
```
security profile_name vrf vrf_name
```

### GRE
---
1. Создаем туннельный интерфейс, прописываем адрес, mtu и параметры туннеля:
```
interface tunnel.0
	ip address 
	ip mtu 1400
	ip tunnel source destination mode gre
```

2. Прописываем маршрут с nexthop на другом конце туннеля:
```
ip route 192.168.1.0/24 10.0.0.2
```

### OSPF
---
1. Прописываем все необходимые параметры:
```
router ospf 1
	ospf router-id 1.1.1.1
	network 172.16.1.0 0.0.0.255 area 0
	network 10.0.0.0 0.0.0.255 area 0
	passive-interface default
	no passive-interface tunnel.0
```

2. Если тип сети не поддерживает мультикастную адресную рассылку (нужен point-to-point/point-to-multipoint), то необходимо будет указать соседей вручную:
```
router ospf 1
	neighbor ip
```

### VRRP
---
1. Создаем vrrp-правило? с любым id и интерфейсом в сторону локальной сети (owner указывается, если нужно явно указать владельца ip = master):
```
router vrrp vrrp_id interface_name [owner]
```

2. Указываем приоритет (по дефолту 100):
```
priority 255 (если овнер)
```

3. Включаем vrrp:
```
enable
```
