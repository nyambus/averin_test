apt-get update && apt-get install bind bind-utils -y

OPTIONS

systemctl enable --now bind

`nano /etc/net/ifaces/ens5/resolv.conf`:
```
search au.team
nameserver 127.0.0.1
```

cp /var/lib/bind/etc/zone/{localdomain,champ.db}

chown named. /var/lib/bind/etc/zone/au-team.db
chmod 600 /var/lib/bind/etc/zone/au-team.db

ZONES

named-checkconf -z

systemctl restart bind

cp /var/lib/bind/etc/zone/{127.in-addr.arpa,1.db}
cp /var/lib/bind/etc/zone/{127.in-addr.arpa,2.db}

chown named. /var/lib/bind/etc/zone/{1,2}.db
chmod 600 /var/lib/bind/etc/zone/{1,2}.db

PTR ZONES

named-checkconf -z

systemctl restart bind

Проверять:
```
named-checkconf -z
host
dig
nslookup
```

