# Docker Desktop + WSL + LDAP + Aplicación Web

## Objetivo

Implementar una arquitectura moderna utilizando Docker Desktop y WSL para desplegar un servidor LDAP y una aplicación web PHP autenticada mediante LDAP.

---

# Arquitectura

Windows
│
Docker Desktop
│
WSL Ubuntu
│
├── Contenedor LDAP
├── Contenedor Web PHP
└── Red Docker

---

# docker-compose.yml

```yaml
version: '3.9'

services:

  ldap:
    image: osixia/openldap:latest
    container_name: ldap_server
    environment:
      LDAP_ORGANISATION: "Empresa"
      LDAP_DOMAIN: "empresa.local"
      LDAP_ADMIN_PASSWORD: "admin123"
    ports:
      - "389:389"

  web:
    build: ./web
    container_name: web_app
    ports:
      - "8080:80"
    depends_on:
      - ldap
# Dockerfile
FROM php:8.2-apache

RUN apt-get update && apt-get install -y \
    libldap2-dev

RUN docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/

RUN docker-php-ext-install ldap

COPY . /var/www/html/

#Explicación de Redes Docker

Docker Compose crea automáticamente una red interna para permitir comunicación
entre contenedores.

La aplicación web se conecta al LDAP utilizando:
ldap_connect("ldap://ldap");
No se utiliza localhost porque cada contenedor tiene su propia red aislada.

#Ventajas frente al entorno local
Servicios aislados
Mejor organización
Arquitectura moderna
Fácil despliegue
Menos conflictos
Cercano a producción

#Evidencias
docker ps funcionando
Login LDAP funcionando
Logs Docker funcionando
Contenedores activos

#Conclusión

Se implementó exitosamente una arquitectura moderna utilizando Docker Desktop,
WSL, LDAP y contenedores Docker para autenticación web.
