# repositoriop62
# Configuración de Contenedores DNS y Cliente en Docker

Guía para crear una red personalizada y configurar un servidor DNS (`bind9`) y un cliente (`cliente`) en Docker.

---

## 1. Crear y Configurar la Red Docker

Para que los contenedores puedan comunicarse, crea una red llamada `adrian_subnet`:

```bash
docker network create --driver bridge --subnet 172.29.0.0/16 --gateway 172.29.8.254 adrian_subnet
