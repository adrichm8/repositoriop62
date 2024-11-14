# repositoriop62
## 1. Crear y Configurar la Red Docker

Para que los contenedores `bind9` y `cliente` puedan comunicarse, primero crea una red llamada `adrian_subnet`:

```bash
docker network create --driver bridge --subnet 172.29.0.0/16 --gateway 172.29.8.254 adrian_subnet
