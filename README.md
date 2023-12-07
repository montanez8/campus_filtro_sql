## carlos Arturo Montañez Rodriguez

## Diagrama Entidad Relacion


## Diagrama Relacional


## Creacion de la base de datos

```sql
CREATE DATABASE sql_filtro
    DEFAULT CHARACTER SET = 'utf8mb4';
        
```

## Creacion de Tablas

```sql
CREATE TABLE
    `zonas`(
        `id_zona` INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
        `nombre` VARCHAR(255) NOT NULL
    );

CREATE TABLE
    `estaciones`(
        `id_estacion` INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
        `nombre` VARCHAR(255) NOT NULL
    );

CREATE TABLE
    `conductores`(
        `id_conductor` INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
        `nombre` VARCHAR(45) NOT NULL,
        `apellido` VARCHAR(45) NOT NULL
    );

CREATE TABLE
    `rutas_conductor`(
        `id_conductor` INT NOT NULL,
        `id_zona` INT NOT NULL,
        `id_ruta` INT NOT NULL,
        `id_bus` INT NOT NULL,
        `id_dia` INT NOT NULL
    );

CREATE TABLE
    `rutas`(
        `id_ruta` INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
        `nombre` VARCHAR(255) NOT NULL,
        `hora` TIME NOT NULL
    );

CREATE TABLE
    `estaciones_rutas`(
        `id_ruta` INT NOT NULL,
        `id_estacion` INT NOT NULL
    );

CREATE TABLE
    `dias_laborales`(
        `id_dia` INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
        `nombre` VARCHAR(255) NOT NULL
    );

CREATE TABLE
    `buses`(
        `id_bus` INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
        `nombre` VARCHAR(255) NOT NULL
    );

ALTER TABLE `rutas_conductor`
ADD
    CONSTRAINT `rutas_conductor_id_zona_foreign` FOREIGN KEY(`id_zona`) REFERENCES `zonas`(`id_zona`);

ALTER TABLE `rutas_conductor`
ADD
    CONSTRAINT `rutas_conductor_id_bus_foreign` FOREIGN KEY(`id_bus`) REFERENCES `buses`(`id_bus`);

ALTER TABLE `estaciones_rutas`
ADD
    CONSTRAINT `estaciones_rutas_id_ruta_foreign` FOREIGN KEY(`id_ruta`) REFERENCES `rutas`(`id_ruta`);

ALTER TABLE `rutas_conductor`
ADD
    CONSTRAINT `rutas_conductor_id_conductor_foreign` FOREIGN KEY(`id_conductor`) REFERENCES `conductores`(`id_conductor`);

ALTER TABLE `rutas_conductor`
ADD
    CONSTRAINT `rutas_conductor_id_ruta_foreign` FOREIGN KEY(`id_ruta`) REFERENCES `rutas`(`id_ruta`);

ALTER TABLE `rutas_conductor`
ADD
    CONSTRAINT `rutas_conductor_id_dia_foreign` FOREIGN KEY(`id_dia`) REFERENCES `dias_laborales`(`id_dia`);

ALTER TABLE `estaciones_rutas`
ADD
    CONSTRAINT `estaciones_rutas_id_estacion_foreign` FOREIGN KEY(`id_estacion`) REFERENCES `estaciones`(`id_estacion`);


```

## Insercion de datos.

```sql
-- TAbla estaciones
INSERT INTO estaciones
VALUES (1, 'colseguros'), (2, 'clinica chicamocha'), (3, 'plaza guarin'), (4, 'maga mall'), (5, 'uis'), (6, 'udi'), (7, 'santo tomas'), (8, 'Boulevard Santander'), (9, 'Búcaros'), (10, 'Rosita'), (11, 'Puerta del Sol'), (12, 'Cacique'), (13, 'Plaza Satélite'), (14, 'La Sirena'), (15, 'Provenza '), (16, 'Fontana'), (17, 'Gibraltar '), (18, 'Terminal'), (19, 'Terminal'), (20, 'Plaza Real');

-- Tabla rutas

INSERT INTO `rutas` VALUES (1,'Universidades','02:00:00'),(2,'Café Madrid','02:15:00'),(3,'Cacique','01:45:00'),(4,'Diamantes','01:50:00'),(5,'Terminal','02:00:00'),(6,'Prado','01:30:00'),(7,'Cabecera','01:30:00'),(8,'Ciudadela','02:00:00'),(9,'Punta Estrella','02:30:00'),(10,'Niza','02:45:00'),(11,'Autopista','02:15:00'),(12,'Lagos','02:15:00'),(13,'Centro Florida','02:30:00');

-- Tabla Buses
INSERT INTO `buses` VALUES (1,'XVH345'),(2,'XDL965'),(3,'XFG847'),(4,'XRJ452'),(5,'XDF459'),(6,'XET554'),(7,'XKL688'),(8,'XXL757');

-- tabla conductores
INSERT INTO `conductores` VALUES (1,'andres manuel','lopez obrador'),(2,'nicolas ','maduro'),(3,'alberto','fernandez'),(4,'luis inacio','lula silva'),(5,'grabiel ','boric'),(6,'miguel','diaz canel'),(7,'daniel ','ortega'),(8,'gustavo','petro'),(9,'luis ','arce'),(10,'xiomara ','castro');

-- Tabla estaciones_rutas 
INSERT INTO `dbclient_export_1701912269413` VALUES (1,1),(1,2),(1,3),(1,4),(1,5),(1,6),(1,7),(3,8),(3,9),(3,10),(3,11),(3,12),(4,13),(4,14),(4,15),(5,16),(5,17),(8,18),(8,19),(8,20);

-- tabla rutas 
INSERT INTO `dbclient_export_1701912050980` VALUES (1,'Universidades','02:00:00'),(2,'Café Madrid','02:15:00'),(3,'Cacique','01:45:00'),(4,'Diamantes','01:50:00'),(5,'Terminal','02:00:00'),(6,'Prado','01:30:00'),(7,'Cabecera','01:30:00'),(8,'Ciudadela','02:00:00'),(9,'Punta Estrella','02:30:00'),(10,'Niza','02:45:00'),(11,'Autopista','02:15:00'),(12,'Lagos','02:15:00'),(13,'Centro Florida','02:30:00');

-- Tabla rutas_conductores

INSERT INTO `rutas conductores` VALUES (5,1,1,1,1),(5,1,1,1,2),(5,1,1,3,3),(5,1,1,3,4),(5,1,2,5,5),(5,1,2,5,6),(5,1,2,5,7),(3,4,4,5,1),(3,4,4,6,2),(3,4,4,1,3),(3,4,5,1,4),(3,4,5,3,5),(3,4,5,3,6),(3,4,5,3,7),(10,5,10,3,1),(10,5,10,3,2),(10,5,10,5,3),(10,5,10,5,4),(10,5,10,4,5),(10,5,11,7,6),(10,5,11,7,7),(7,5,11,7,1),(7,5,11,7,1),(6,5,12,7,3),(6,5,12,7,4),(6,5,12,7,5),(6,5,12,6,6),(6,5,12,6,7);
```

# Consultas

- 1. Cantidad de Paradas por Ruta

```sql
SELECT r.nombre AS ruta, COUNT(er.id_estacion) AS cantidad_paradas
FROM rutas r
JOIN estaciones_rutas er ON r.id_ruta = er.id_ruta
GROUP BY r.nombre;


```bash
+---------------+------------------+
| ruta          | cantidad_paradas |
+---------------+------------------+
| Universidades |                7 |
| Cacique       |                5 |
| Diamantes     |                3 |
| Terminal      |                2 |
| Ciudadela     |                3 |
+---------------+------------------+
```
- 2. Nombre de las Paradas de la Ruta Universidades

```sql
SELECT es.nombre from estaciones es
INNER JOIN estaciones_rutas er ON es.id_estacion = er.id_estacion
INNER JOIN rutas r ON er.id_ruta = r.id_ruta
WHERE r.id_ruta = 1;
```
### Resultado
```bash
+--------------------+
| nombre             |
+--------------------+
| colseguros         |
| clinica chicamocha |
| plaza guarin       |
| maga mall          |
| uis                |
| udi                |
| santo tomas        |
+--------------------+
```
- 3. Nombres de las Rutas No Programadas

```sql
SELECT r.nombre
FROM rutas r
LEFT JOIN rutas_conductor rc ON r.id_ruta = rc.id_ruta
WHERE rc.id_ruta IS NULL;

```
### Resultado
```bash
+----------------+
| nombre         |
+----------------+
| Cacique        |
| Prado          |
| Cabecera       |
| Ciudadela      |
| Punta Estrella |
| Centro Florida |
+----------------+
```
- 4. Rutas Pogramadas sin conductor asignado

```sql
SELECT DISTINCT c.nombre FROM conductores c
LEFT JOIN rutas_conductor rc ON c.id_conductor = rc.id_conductor;

```
### Resultado
```bash
+----------------+
| nombre         |
+----------------+
| Universidades  |
| Café Madrid    |
| Cacique        |
| Diamantes      |
| Terminal       |
| Prado          |
| Cabecera       |
| Ciudadela      |
| Punta Estrella |
| Niza           |
| Autopista      |
| Lagos          |
| Centro Florida |
+----------------+
```
- 5 Conductores No Asignados a la Programación
```sql
SELECT DISTINCT c.nombre FROM conductores c
LEFT JOIN rutas_conductor rc ON c.id_conductor = rc.id_conductor;

```
### Resultado
```bash
+---------------+
| nombre        |
+---------------+
| andres manuel |
| nicolas       |
| alberto       |
| luis inacio   |
| grabiel       |
| miguel        |
| daniel        |
| gustavo       |
| luis          |
| xiomara       |
+---------------+
```
- 6 Buses No asignados a la Programación
```sql
SELECT b.nombre
FROM buses b
LEFT JOIN rutas_conductor rc ON b.id_bus = rc.id_bus
WHERE rc.id_bus IS NULL;


```
### Resultado
```bash
+--------+
| nombre |
+--------+
| XDL965 |
| XXL757 |
+--------+
```

- 7 Zonas NO Programadas
```sql
SELECT z.nombre
FROM zonas z
LEFT JOIN rutas_conductor rc ON z.id_zona = rc.id_zona
WHERE rc.id_zona IS NULL;


```
### Resultado
```bash
+-------------+
| nombre      |
+-------------+
| Sur         |
| Oriente     |
| Girón       |
| Piedecuesta |
+-------------+
```
- 8 Programación asignada a cada conductor (Conductor, Ruta y Día)
```sql
SELECT c.nombre, c.apellido, r.nombre AS ruta, dl.nombre AS dia
FROM rutas_conductor rc
JOIN conductores c ON rc.id_conductor = c.id_conductor
JOIN rutas r ON rc.id_ruta = r.id_ruta
JOIN dias_laborales dl ON rc.id_dia = dl.id_dia;



```
### Resultado
```bash
+----------+------------+---------------+-----------+
| nombre   | apellido   | ruta          | dia       |
+----------+------------+---------------+-----------+
| grabiel  | boric      | Universidades | lunes     |
| alberto  | fernandez  | Diamantes     | lunes     |
| xiomara  | castro     | Niza          | lunes     |
| daniel   | ortega     | Autopista     | lunes     |
| daniel   | ortega     | Autopista     | lunes     |
| grabiel  | boric      | Universidades | martes    |
| alberto  | fernandez  | Diamantes     | martes    |
| xiomara  | castro     | Niza          | martes    |
| grabiel  | boric      | Universidades | miercoles |
| alberto  | fernandez  | Diamantes     | miercoles |
| xiomara  | castro     | Niza          | miercoles |
| miguel   | diaz canel | Lagos         | miercoles |
| grabiel  | boric      | Universidades | jueves    |
| alberto  | fernandez  | Terminal      | jueves    |
| xiomara  | castro     | Niza          | jueves    |
| miguel   | diaz canel | Lagos         | jueves    |
| grabiel  | boric      | Café Madrid   | viernes   |
| alberto  | fernandez  | Terminal      | viernes   |
| xiomara  | castro     | Niza          | viernes   |
| miguel   | diaz canel | Lagos         | viernes   |
| grabiel  | boric      | Café Madrid   | sabado    |
| alberto  | fernandez  | Terminal      | sabado    |
| xiomara  | castro     | Autopista     | sabado    |
| miguel   | diaz canel | Lagos         | sabado    |
| grabiel  | boric      | Café Madrid   | domingo   |
| alberto  | fernandez  | Terminal      | domingo   |
| xiomara  | castro     | Autopista     | domingo   |
| miguel   | diaz canel | Lagos         | domingo   |
+----------+------------+---------------+-----------+
```

- 9 Programación asignada a conductores que hacen rutas de más de dos horas
```sql
SELECT c.nombre, c.apellido, r.nombre AS ruta
FROM rutas_conductor rc
JOIN conductores c ON rc.id_conductor = c.id_conductor
JOIN rutas r ON rc.id_ruta = r.id_ruta
WHERE TIME_TO_SEC(r.hora) > 7200;


```
### Resultado
```bash
+----------+------------+--------------+
| nombre   | apellido   | ruta         |
+----------+------------+--------------+
| grabiel  | boric      | Café Madrid  |
| grabiel  | boric      | Café Madrid  |
| grabiel  | boric      | Café Madrid  |
| xiomara  | castro     | Niza         |
| xiomara  | castro     | Niza         |
| xiomara  | castro     | Niza         |
| xiomara  | castro     | Niza         |
| xiomara  | castro     | Niza         |
| xiomara  | castro     | Autopista    |
| xiomara  | castro     | Autopista    |
| daniel   | ortega     | Autopista    |
| daniel   | ortega     | Autopista    |
| miguel   | diaz canel | Lagos        |
| miguel   | diaz canel | Lagos        |
| miguel   | diaz canel | Lagos        |
| miguel   | diaz canel | Lagos        |
| miguel   | diaz canel | Lagos        |
+----------+------------+--------------+

```
- 10 Nombres de Zonas y cantidad de rutas que tienen programadas (Contar)
```sql
SELECT z.nombre, COUNT(rc.id_ruta) AS cantidad_rutas_programadas
FROM zonas z
LEFT JOIN rutas_conductor rc ON z.id_zona = rc.id_zona
GROUP BY z.nombre;


```
### Resultado
```+---------------+----------------------------+
| nombre        | cantidad_rutas_programadas |
+---------------+----------------------------+
| Norte         |                          7 |
| Sur           |                          0 |
| Oriente       |                          0 |
| Occidente     |                          7 |
| Floridablanca |                         14 |
| Girón         |                          0 |
| Piedecuesta   |                          0 |
+---------------+----------------------------+
```