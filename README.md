
# PROYECTO ACCESO A DATOS

Proyecto final de ADD - API NBA


## Desarrollo y comandos utilizados

#### 1. Se construyen y se ejecutan los contenedores docker

```bash
~/add-env/api-nba$ docker-compose up -d
~/add-env/mysql-rds$ docker-compose up -d
```
#### 2. Acceder al contenedor donde esta montada la API

```bash
~/add-env/api-nba$ docker-compose exec web bash
/code$
```
#### 3. Se crea un nuevo proyecto symfony con nombre api-nba

```bash
/code$ symfony new api-nba --version=4.4 -full -no-git
```
#### 4. Pasar todos los fichero del proyecto symfony al repositorio raiz (/code)

```bash
/code$ cd api-nba/
/code$ cp * ../
/code$ rm -r api-nba/
```
#### 5. Ahora creamos 2 repositorios mas, files donde se guardan los ficheros con la informacion de la base de datos y scripts donde se guardan los programas python

```bash
/code$ mkdir files/
/code$ mkdir scripts/
```
#### 6. Pasamos a la creación de la base datos vacia en MySQL

```bash
/code/files$ mysql -h add-dbms -u root -pdbrootpass < nba_2022-02-02.sql
```

#### 7. Y ahora introduciremos los datos en nuestro MySQL mediante los programas Python

MUY IMPORTANTE❗ Los siguientes comandos hay que
ejecutarlos desde el repositorio raiz /code para el correcto
funcionamiento de los programas

```bash
/code$ python3 scripts/load_equipos.py
/code$ python3 scripts/load_jugadores.py
/code$ python3 scripts/load_estadisticas.py
/code$ python3 scripts/load_partidos.py
```
#### 8. Y por ultimo, mediante Doctrine, generamos nuestras Entities a partir de las tablas de nuestra base de datos

```bash
/code$ php bin/console doctrine:mapping:convert annotation src/Entity/ --from-database
```
## Endpoints generados

#### Equipos
Listar todos los equipos con toda su información

```http
/equipos
```
![App Screenshot](https://raw.githubusercontent.com/pepbon09/ProyectoADD/main/img/endpoint1.png)

#### Equipo
Listar un equipo, dado su nombre, con toda su información

```http
/equipos/{nombre}
```
![App Screenshot](https://raw.githubusercontent.com/pepbon09/ProyectoADD/main/img/endpoint2.png)

| Parametros | Tipo     | Descripcion                                 |
| :--------- | :------- | :------------------------------------------ |
| `nombre`   | `string` | **Required**. El nombre del equipo a buscar |

#### Jugadores de cada equipo
Listar para cada equipo, todos sus jugadores

```http
/equipo/jugadores
```
![App Screenshot](https://raw.githubusercontent.com/pepbon09/ProyectoADD/main/img/endpoint3.png)
#### Jugadores de un equipo
Listar para un equipo, dado su nombre, todos sus jugadores

```http
/equipo/jugadores/{nombre}
```
![App Screenshot](https://raw.githubusercontent.com/pepbon09/ProyectoADD/main/img/endpoint4.png)
| Parametros | Tipo     | Descripcion                                 |
| :--------- | :------- | :------------------------------------------ |
| `nombre`   | `string` | **Required**. El nombre del equipo a buscar |

#### Jugadores
Listar todos los jugadores con toda su información

```http
/jugadores
```
![App Screenshot](https://raw.githubusercontent.com/pepbon09/ProyectoADD/main/img/endpoint5.png)
#### Jugador
Listar un jugador, dado su nombre, con toda su información

```http
/jugadores/{nombre}
```
![App Screenshot](https://raw.githubusercontent.com/pepbon09/ProyectoADD/main/img/endpoint6.png)

| Parametros | Tipo     | Descripcion                                  |
| :--------- | :------- | :------------------------------------------- |
| `nombre`   | `string` | **Required**. El nombre del jugador a buscar |

#### Fisico Jugador
Listar un jugador, dado su nombre, su altura en cm., peso en kg. y posición

```http
/jugadores/fisico/{nombre}
```
![App Screenshot](https://raw.githubusercontent.com/pepbon09/ProyectoADD/main/img/endpoint7.png)

| Parametros | Tipo     | Descripcion                                  |
| :--------- | :------- | :------------------------------------------- |
| `nombre`   | `string` | **Required**. El nombre del jugador a buscar |

#### Estadisticas de un jugador
Listar un jugador, dado su nombre, todas sus estadisticas por temporada

```http
/estadisticas/jugador/{nombre}
```
![App Screenshot](https://raw.githubusercontent.com/pepbon09/ProyectoADD/main/img/endpoint8.png)

| Parametros | Tipo     | Descripcion                                  |
| :--------- | :------- | :------------------------------------------- |
| `nombre`   | `string` | **Required**. El nombre del jugador a buscar |

#### Medias de un jugador
Listar un jugador, dado su nombre, la media de todas sus estadisticas en la base de datos

```http
/estadisticas/jugador/{nombre}/avg
```
![App Screenshot](https://raw.githubusercontent.com/pepbon09/ProyectoADD/main/img/endpoint9.png)

| Parametros | Tipo     | Descripcion                                  |
| :--------- | :------- | :------------------------------------------- |
| `nombre`   | `string` | **Required**. El nombre del jugador a buscar |

#### Resultados como local
Listar para un equipo, dado su nombre, todos sus resultados que ha jugado como local

```http
/partidos/resultados/local/{nombre}
```
![App Screenshot](https://raw.githubusercontent.com/pepbon09/ProyectoADD/main/img/endpoint10.png)

| Parametros | Tipo     | Descripcion                                 |
| :--------- | :------- | :------------------------------------------ |
| `nombre`   | `string` | **Required**. El nombre del equipo a buscar |

#### Resultados como visitante
Listar para un equipo, dado su nombre, todos sus resultados que ha jugado como visitante

```http
/partidos/resultados/visitante/{nombre}
```
![App Screenshot](https://raw.githubusercontent.com/pepbon09/ProyectoADD/main/img/endpoint11.png)

| Parametros | Tipo     | Descripcion                                 |
| :--------- | :------- | :------------------------------------------ |
| `nombre`   | `string` | **Required**. El nombre del equipo a buscar |

#### Media de puntos recibido como local
Listar para un equipo, dado su nombre, la media de puntos recibidos como local

```http
/partidos/resultados/media/local/{nombre}
```
![App Screenshot](https://raw.githubusercontent.com/pepbon09/ProyectoADD/main/img/endpoint12.png)

| Parametros | Tipo     | Descripcion                                 |
| :--------- | :------- | :------------------------------------------ |
| `nombre`   | `string` | **Required**. El nombre del equipo a buscar |

#### Media de puntos recibidos como visitante
Listar para un equipo, dado su nombre, la media de puntos recibidos como visitante

```http
/partidos/resultados/media/visitante/{nombre}
```
![App Screenshot](https://raw.githubusercontent.com/pepbon09/ProyectoADD/main/img/endpoint13.png)

| Parametros | Tipo     | Descripcion                                 |
| :--------- | :------- | :------------------------------------------ |
| `nombre`   | `string` | **Required**. El nombre del equipo a buscar |


## Autor

Pep Bonafont Chiner [@pepbon09](https://github.com/pepbon09)
