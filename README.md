# repositoriop62
## 1. Crear y configurar la red docker

Para que los contenedores `bind9` y `cliente` puedan comunicarse, primero crea una red llamada `adrian_subnet`:

```
docker network create --driver bridge --subnet 172.29.0.0/16 --gateway 172.29.8.254 adrian_subnet
```
tendría que verse así

```
{
  "Name": "adrian_subnet",
  "IPAM": {
    "Config": [
      {
        "Subnet": "172.29.0.0/16",
        "Gateway": "172.29.8.254"
      }
    ]
  }
}
```
# Generar archivo docker-compose.yml
Este archivo docker-compose.yml levantará los contenedores bind9 y cliente en la red personalizada. Crea un archivo docker-compose.yml con el siguiente contenido
```
services:
  bind9:
    container_name: bind9-adri
    image: internetsystemsconsortium/bind9:9.18
    ports:
      - "54:53/tcp"
      - "54:53/udp"
    volumes:
      - ./conf:/etc/bind
      - ./zonas:/var/lib/bind
    networks:
      adrian_subnet:
        ipv4_address: 172.19.0.1

  cliente:
    container_name: cliente
    image: alpine
    tty: true
    dns:
      - 172.19.0.1
    networks:
      adrian_subnet:
        ipv4_address: 172.19.0.2

networks:
  adrian_subnet:
    driver: bridge
    ipam:
      config:
        - subnet: 172.29.0.0/16
          gateway: 172.29.8.254
```
# Iniciar contenedores con docker compose
Una vez que tengas tu archivo docker-compose.yml, inicia los contenedores con el siguiente comando:
```
docker-compose up -d
```
# Verificación de la red
Verifica que los contenedores estén correctamente conectados a la red adrian_subnet
```
docker network inspect adrian_subnet
```
La salida debe mostrar que los contenedores bind9 y cliente están conectados a la red adrian_subnet con sus direcciones IP asignadas.
# Configuración de Archivos de DNS
  # Crear Archivos de Configuración de bind9
  Primero se hara para la zona local generando el archivo named.conf.local
```
zone "33asircastelao.int" {
    type master;
    file "/var/lib/bind/db.33asircastelao.int";
    allow-query { any; };
};
```
Posteriormente para zona predeterminada generando el archivo named.conf.default-zones
```
zone "." {
    type hint;
    file "/usr/share/dns/root.hints";
};

zone "localhost" {
    type master;
    file "/etc/bind/db.local";
};

zone "127.in-addr.arpa" {
    type master;
    file "/etc/bind/db.127";
};
```
y finalmente se generará el archivo named.conf
```
include "/etc/bind/named.conf.options";
include "/etc/bind/named.conf.local";
```
# Verificar  DNS desde el contenedor cliente
Accede al contenedor cliente y realiza una consulta DNS utilizando el comando dig:
```
docker exec -it cliente sh
dig @172.19.0.1 33asircastelao.int
```
Tendría que dar una salida, que sería la que está a continuación
```
; <<>> DiG 9.18.27 <<>> 33asircastelao.int
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 20302
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1
;; QUESTION SECTION:
;33asircastelao.int.		IN	A
;; ANSWER SECTION:
33asircastelao.int.		3600	IN	A	172.19.0.1
```
# Realizar ping a dominio externo
Se puede hacer un ping a un dominio externo para verificar que la conexión a Internet funciona
```
ping google.com
```
Debería salir que la máquina tiene acceso a red externa
