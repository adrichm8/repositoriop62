# repositoriop62
## 1. Creación de la Red en Docker

Primero, crea y verifica la red `bind9_subnet` con el siguiente comando:

docker network inspect bind9_subnet

css
Copiar código

Esto te mostrará la configuración de la red, que debe verse algo similar a lo siguiente:

[ { "Name": "bind9_subnet", "Id": "ef5c911f15e82b5a9b59c0d1b8bc9fc49746ab040ae542b8f79b1e12d2312e0e", "Created": "2024-10-25T17:50:55.279860915+02:00", "Scope": "local", "Driver": "bridge", "EnableIPv6": false, "IPAM": { "Driver": "default", "Options": {}, "Config": [ { "Subnet": "192.168.10.0/16", "IPRange": "192.168.15.0/24", "Gateway": "192.168.15.254" } ] }, "Internal": false, "Attachable": false, "Ingress": false, "ConfigFrom": { "Network": "" }, "ConfigOnly": false, "Containers": { "e704f2970d1441762ab7fa0890d8e6666447628f7e98dd038eb3db82036bf27a": { "Name": "cliente", "EndpointID": "09f0ccbe06e3de2d88a4ada147511d94eeb8ae54319f37a396f358ee84847d99", "MacAddress": "02:42:ac:1c:05:02", "IPv4Address": "192.168.15.2/16", "IPv6Address": "" } }, "Options": {}, "Labels": {} } ]

perl
Copiar código
