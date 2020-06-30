![Texto alternativo](https://github.com/MoisesTapia/Hack-Stack/blob/master/img/hack_stak.png)

[![Channel](https://img.shields.io/badge/channel-YouTube-red)](https://www.youtube.com/channel/UCiuZK5geN3OCGeBxuXMfHEQ)
[![Facebook](https://img.shields.io/badge/Facebook-DartSecurity-blue)](https://www.facebook.com/dartasec/)<br>

[![Docker](https://img.shields.io/badge/Docker-19.03.8-blue)](https://www.docker.com/)
[![Postgre](https://img.shields.io/badge/PostgreSQL-latest-orange)](https://hub.docker.com/_/postgres)
[![metasploit](https://img.shields.io/badge/Metasploit-5-success)](https://hub.docker.com/r/metasploitframework/metasploit-framework)
[![grafana](https://img.shields.io/badge/Grafana-7.0.4-orange)](https://hub.docker.com/r/grafana/grafana)
[![Dradis](https://img.shields.io/badge/Dradis-latest-yellow)](https://hub.docker.com/r/grafana/grafana)


# Enviroment

## Acceso
- **"Grafana URL: Localhost:3000"**
- **"Dradis URL: Localhost:3010"**

## Descarga
```bash
git clone https://github.com/MoisesTapia/Hack-Stack
cd Hack-Stack
```
## Carpetas

* [x] Crea una carpeta en dentro de la misma carpeta del proyecto `.msf4`
  - [ ] `mkdir .msf4`
  - [ ] Cabiamos `sudo chown 1000 .msf4`
* [x] Crea una carpeta en dentro de la misma carpeta del proyecto `db`
  - [ ] `mkdir db`
* [x] Crea una carpeta en dentro de la misma carpeta del proyecto `grafana`
  - [ ] `mkdir grafana`
  - [ ] Cambiamos `sudo chown 1000 grafana`
 
## Dockerfile

En la crpeta del proyecto esta el **`docker-compose.yml`** es importante no cambiar nada de su contenido si no tiene conocimientos de docker en caso de que si y requiera cambiar el nombre de la **base de datos** y la **contraseña** lo puede realizar dentro de este archivo en las variables de entorno

```bash
      environment: 
        - POSTGRES_PASSWORD=msfpassword
        - POSTGRES_DB=msf
```
al igual que en el **``DATABASE_URL``** tienen que coincidir las contraseñas


# Levantando el Lab

## Docker-Compose
Drento de la carpeta de ``Hack-Stack`` ejecutamos el siguiente comando

```bash
docker-compose up -d
```
![Texto alternativo](https://github.com/MoisesTapia/Hack-Stack/blob/master/img/docker-compose-up.png)
cuando levante el stack vamos a ejecutar el comando ``docker-compose ps`` para verificar que tenemos la siguiente salida
```bash
   Name                 Command               State           Ports         
----------------------------------------------------------------------------
grafana      /run.sh                          Up      0.0.0.0:3000->3000/tcp
metasploit   docker/entrypoint.sh ./msf ...   Up      0.0.0.0:4444->4444/tcp
postgres     docker-entrypoint.sh postgres    Up      0.0.0.0:5432->5432/tcp

```
Es importante verificar que los 3 servicios estan corriendo de manera correcta

# Stack
Configuracion de los ambientes levantados

## Grafana
Para entrar al panel de monitoreo de **_Grafana_** solamente escribimos en nuestro navegador **``localhost:3000``**
las credenciales de acceso son por default:
- admin
- admin <br>

la contraseña te pedira cambiarla por una nueva

## Coneccion a base de datos

Cuando estemos dentro de **_Grafana_** en el menu vamos a ir a:
![Texto alternativo](https://github.com/MoisesTapia/Hack-Stack/blob/master/img/new_datasources.png)

Damos click y nos mostrara muchas datasources pero nosotros vamos a seleccionar 

![Texto alternativo](https://github.com/MoisesTapia/Hack-Stack/blob/master/img/postgresql_data.png)

Nos mostrar un tablero con la configuracion de la conexion y la configuracion que ponderemos en las siguiente:

![Texto alternativo](https://github.com/MoisesTapia/Hack-Stack/blob/master/img/Conf_datasource.png)

Las credenciales son las que se encuentran dentro de el archivo **docker-compose.yml**


```bash
USER= postgres
PASSWORD=msfpassword
DB=msf
```
Despues de la configuracion ya podemos dar click en **save & test** nos debe mostrar un mensaje de la siguiente manera

![Texto alternativo](https://github.com/MoisesTapia/Hack-Stack/blob/master/img/save_and_test.png)

## Graficando

Desde este momento ya podemos graficar los datos que se guarden de nuestros escaneos en postgres en grafana

![Texto alternativo](https://github.com/MoisesTapia/Hack-Stack/blob/master/img/Dashboards.png)

# Metasploit

Pra ingresar y usar **_metasploit_** es una manera muy facil tenemos que ejecutar el siguiente comando:

```bash
docker-compose exec metasploit docker/entrypoint.sh ./msfconsole -r docker/msfconsole.rc -y $APP_HOME/config/database.yml
```
## **_Es importante que copiemos la line completa para que se ejecute de manera correcta_**

ya que la ajecutamos podemos ver la siguiente salida:

![Texto alternativo](https://github.com/MoisesTapia/Hack-Stack/blob/master/img/metasploit_connection_up2.png)

## Saliendo

Para salir del contendore de metasploit solo basta con escribir **exit** y listo

# Deteniendo servicios

para detener el Stack solo tenemos que escribir las siguientes lineas
```bash
docker-compose stop
```
