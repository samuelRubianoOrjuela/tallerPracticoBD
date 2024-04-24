### Consultas sobre una tabla
---

1. Devuelve un listado con el primer apellido, segundo apellido y el nombre de todos los alumnos. El listado deberá estar ordenado alfabéticamente de menor a mayor por el primer apellido, segundo apellido y nombre.
    ```sql
    SELECT CONCAT(apellido1, ' ', apellido2, ' ', nombre) AS nombre_alumno
    FROM persona
    WHERE tipo = 'alumno';

    +-----------------------------+
    | nombre_alumno               |
    +-----------------------------+
    | Sánchez Pérez Salvador      |
    | Saez Vega Juan              |
    | Heller Pagac Pedro          |
    | Koss Bayer José             |
    | Strosin Turcotte Ismael     |
    | Herzog Tremblay Ramón       |
    | Herman Pacocha Daniel       |
    | Lakin Yundt Inma            |
    | Gutiérrez López Juan        |
    | Domínguez Guerrero Antonio  |
    | Hernández Martínez Irene    |
    | Gea Ruiz Sonia              |
    +-----------------------------+
    ```
    ---

2. Averigua el nombre y los dos apellidos de los alumnos que no han dado de alta su número de teléfono en la base de datos.
    ```sql
    SELECT CONCAT(nombre, ' ', apellido1, ' ', apellido2) AS nombre_alumno
    FROM persona
    WHERE tipo = 'alumno' AND telefono IS NOT NULL;

    +-----------------------------+
    | nombre_alumno               |
    +-----------------------------+
    | Salvador Sánchez Pérez      |
    | Juan Saez Vega              |
    | José Koss Bayer             |
    | Ramón Herzog Tremblay       |
    | Daniel Herman Pacocha       |
    | Inma Lakin Yundt            |
    | Juan Gutiérrez López        |
    | Antonio Domínguez Guerrero  |
    | Irene Hernández Martínez    |
    | Sonia Gea Ruiz              |
    +-----------------------------+
    ```
    ---

3. Devuelve el listado de los alumnos que nacieron en 1999.
    ```sql
    SELECT CONCAT(nombre, ' ', apellido1, ' ', apellido2) AS nombre_alumno
    FROM persona
    WHERE YEAR(fecha_nacimiento) = 1999;

    +-----------------------------+
    | nombre_alumno               |
    +-----------------------------+
    | Ismael Strosin Turcotte     |
    | Antonio Domínguez Guerrero  |
    +-----------------------------+

    ```
    ---

4. Devuelve el listado de profesores que no han dado de alta su número de teléfono en la base de datos y además su nif termina en K.
    ```sql
    SELECT id, CONCAT(nombre, ' ', apellido1, ' ', apellido2) AS nombre_profesor
    FROM persona
    WHERE tipo = 'profesor' 
    AND telefono IS NOT NULL 
    AND RIGHT (nif, 1) = 'K';

    Empty set (0,00 sec)
    ```
    ---

5. Devuelve el listado de las asignaturas que se imparten en el primer cuatrimestre, en el tercer curso del grado que tiene el identificador 7.
    ```sql
    SELECT id, nombre
    FROM asignatura
    WHERE cuatrimestre = 1 
    AND id_grado = 7;

    +----+----------------------------------------------+
    | id | nombre                                       |
    +----+----------------------------------------------+
    | 52 | Biologia celular                             |
    | 53 | Física                                       |
    | 54 | Matemáticas I                                |
    | 55 | Química general                              |
    | 56 | Química orgánica                             |
    | 62 | Botánica agrícola                            |
    | 63 | Fisiología vegetal                           |
    | 64 | Genética molecular                           |
    | 65 | Ingeniería bioquímica                        |
    | 66 | Termodinámica y cinética química aplicada    |
    | 72 | Bases moleculares del desarrollo vegetal     |
    | 73 | Fisiología animal                            |
    | 74 | Metabolismo y biosíntesis de biomoléculas    |
    | 75 | Operaciones de separación                    |
    | 76 | Patología molecular de plantas               |
    | 77 | Técnicas instrumentales básicas              |
    +----+----------------------------------------------+
    ```
    ---

### Consultas multitabla (Composición interna)
1. Devuelve un listado con los datos de todas las alumnas que se han matriculado alguna vez en el Grado en Ingeniería Informática (Plan 2015).
    ```sql
    SELECT DISTINCT p.*
    FROM persona p
    JOIN alumno_se_matricula_asignatura aa ON p.id = aa.id_alumno
    JOIN asignatura a ON aa.id_asignatura = a.id
    JOIN grado g ON a.id_grado = g.id
    WHERE p.tipo = 'alumno'
    AND p.sexo = 'M' 
    AND g.nombre = 'Grado en Ingeniería Informática (Plan 2015)';

    +----+-----------+--------+------------+-----------+----------+--------------------+-----------+------------------+------+--------+
    | id | nif       | nombre | apellido1  | apellido2 | ciudad   | direccion          | telefono  | fecha_nacimiento | sexo | tipo   |
    +----+-----------+--------+------------+-----------+----------+--------------------+-----------+------------------+------+--------+
    | 19 | 11578526G | Inma   | Lakin      | Yundt     | Almería  | C/ Picos de Europa | 678652431 | 1998-09-01       | M    | alumno |
    | 23 | 64753215G | Irene  | Hernández  | Martínez  | Almería  | C/ Zapillo         | 628452384 | 1996-03-12       | M    | alumno |
    | 24 | 85135690V | Sonia  | Gea        | Ruiz      | Almería  | C/ Mercurio        | 678812017 | 1995-04-13       | M    | alumno |
    +----+-----------+--------+------------+-----------+----------+--------------------+-----------+------------------+------+--------+

    ```
    ---
    
2. Devuelve un listado con todas las asignaturas ofertadas en el Grado en Ingeniería Informática (Plan 2015).
    ```sql
    SELECT DISTINCT a.id, a.nombre
    FROM asignatura a
    JOIN grado g ON a.id_grado = g.id
    WHERE  g.nombre = 'Grado en Ingeniería Informática (Plan 2015)';

    +----+---------------------------------------------------------------------------+
    | id | nombre                                                                    |
    +----+---------------------------------------------------------------------------+
    |  1 | Álgegra lineal y matemática discreta                                      |
    |  2 | Cálculo                                                                   |
    |  3 | Física para informática                                                   |
    |  4 | Introducción a la programación                                            |
    |  5 | Organización y gestión de empresas                                        |
    |  6 | Estadística                                                               |
    |  7 | Estructura y tecnología de computadores                                   |
    |  8 | Fundamentos de electrónica                                                |
    |  9 | Lógica y algorítmica                                                      |
    | 10 | Metodología de la programación                                            |
    | 11 | Arquitectura de Computadores                                              |
    | 12 | Estructura de Datos y Algoritmos I                                        |
    | 13 | Ingeniería del Software                                                   |
    | 14 | Sistemas Inteligentes                                                     |
    | 15 | Sistemas Operativos                                                       |
    | 16 | Bases de Datos                                                            |
    | 17 | Estructura de Datos y Algoritmos II                                       |
    | 18 | Fundamentos de Redes de Computadores                                      |
    | 19 | Planificación y Gestión de Proyectos Informáticos                         |
    | 20 | Programación de Servicios Software                                        |
    | 21 | Desarrollo de interfaces de usuario                                       |
    | 22 | Ingeniería de Requisitos                                                  |
    | 23 | Integración de las Tecnologías de la Información en las Organizaciones    |
    | 24 | Modelado y Diseño del Software 1                                          |
    | 25 | Multiprocesadores                                                         |
    | 26 | Seguridad y cumplimiento normativo                                        |
    | 27 | Sistema de Información para las Organizaciones                            |
    | 28 | Tecnologías web                                                           |
    | 29 | Teoría de códigos y criptografía                                          |
    | 30 | Administración de bases de datos                                          |
    | 31 | Herramientas y Métodos de Ingeniería del Software                         |
    | 32 | Informática industrial y robótica                                         |
    | 33 | Ingeniería de Sistemas de Información                                     |
    | 34 | Modelado y Diseño del Software 2                                          |
    | 35 | Negocio Electrónico                                                       |
    | 36 | Periféricos e interfaces                                                  |
    | 37 | Sistemas de tiempo real                                                   |
    | 38 | Tecnologías de acceso a red                                               |
    | 39 | Tratamiento digital de imágenes                                           |
    | 40 | Administración de redes y sistemas operativos                             |
    | 41 | Almacenes de Datos                                                        |
    | 42 | Fiabilidad y Gestión de Riesgos                                           |
    | 43 | Líneas de Productos Software                                              |
    | 44 | Procesos de Ingeniería del Software 1                                     |
    | 45 | Tecnologías multimedia                                                    |
    | 46 | Análisis y planificación de las TI                                        |
    | 47 | Desarrollo Rápido de Aplicaciones                                         |
    | 48 | Gestión de la Calidad y de la Innovación Tecnológica                      |
    | 49 | Inteligencia del Negocio                                                  |
    | 50 | Procesos de Ingeniería del Software 2                                     |
    | 51 | Seguridad Informática                                                     |
    +----+---------------------------------------------------------------------------+
    ```
    ---
    
3. Devuelve un listado de los profesores junto con el nombre del departamento al que están vinculados. El listado debe devolver cuatro columnas, primer apellido, segundo apellido, nombre y nombre del departamento. El resultado estará ordenado alfabéticamente de menor a mayor por los apellidos y el nombre.
    ```sql
    SELECT p.apellido1, p.apellido2, p.nombre, d.nombre AS nombre_departamento 
    FROM persona p
    JOIN profesor pf ON p.id = pf.id_profesor
    JOIN departamento d ON pf.id_departamento = d.id;

    +------------+------------+-----------+---------------------+
    | apellido1  | apellido2  | nombre    | nombre_departamento |
    +------------+------------+-----------+---------------------+
    | Ramirez    | Gea        | Zoe       | Informática         |
    | Hamill     | Kozey      | Manolo    | Informática         |
    | Schmidt    | Fisher     | David     | Matemáticas         |
    | Kohler     | Schoen     | Alejandro | Matemáticas         |
    | Lemke      | Rutherford | Cristina  | Economía y Empresa  |
    | Fahey      | Considine  | Antonio   | Economía y Empresa  |
    | Spencer    | Lakin      | Esther    | Educación           |
    | Streich    | Hirthe     | Carmen    | Educación           |
    | Ruecker    | Upton      | Guillermo | Educación           |
    | Monahan    | Murray     | Micaela   | Agronomía           |
    | Stiedemann | Morissette | Alfredo   | Química y Física    |
    | Schowalter | Muller     | Francesca | Química y Física    |
    +------------+------------+-----------+---------------------+
    ```
    ---
    
4. Devuelve un listado con el nombre de las asignaturas, año de inicio y año de fin del curso escolar del alumno con nif 26902806M.
    ```sql
    SELECT a.nombre, c.anyo_inicio, c.anyo_fin
    FROM asignatura a
    JOIN alumno_se_matricula_asignatura aa ON a.id = aa.id_asignatura
    JOIN persona p ON aa.id_alumno = p.id
    JOIN curso_escolar c ON aa.id_curso_escolar = c.id
    WHERE p.nif = '26902806M';

    +----------------------------------------+-------------+----------+
    | nombre                                 | anyo_inicio | anyo_fin |
    +----------------------------------------+-------------+----------+
    | Álgegra lineal y matemática discreta   |        2014 |     2015 |
    | Cálculo                                |        2014 |     2015 |
    | Física para informática                |        2014 |     2015 |
    +----------------------------------------+-------------+----------+
    ```
    ---
    
5. Devuelve un listado con el nombre de todos los departamentos que tienen profesores que imparten alguna asignatura en el Grado en Ingeniería Informática (Plan 2015).
    ```sql
    SELECT DISTINCT d.nombre AS nombre_departamento
    FROM departamento d
    JOIN profesor p ON d.id = p.id_departamento
    JOIN asignatura a ON p.id_profesor = a.id_profesor 
    JOIN grado g ON a.id_grado = g.id
    WHERE g.nombre = 'Grado en Ingeniería Informática (Plan 2015)';

    +---------------------+
    | nombre_departamento |
    +---------------------+
    | Informática         |
    +---------------------+
    ```
    ---
    
6. Devuelve un listado con todos los alumnos que se han matriculado en alguna asignatura durante el curso escolar 2018/2019.
    ```sql
    SELECT  DISTINCT p.id, CONCAT(nombre, ' ', apellido1, ' ', apellido2) AS nombre_alumno
    FROM persona p
    JOIN alumno_se_matricula_asignatura aa ON p.id = aa.id_alumno
    JOIN curso_escolar c ON aa.id_curso_escolar = c.id
    WHERE c.anyo_inicio = 2018;

    +----+----------------------------+
    | id | nombre_alumno              |
    +----+----------------------------+
    | 19 | Inma Lakin Yundt           |
    | 23 | Irene Hernández Martínez   |
    | 24 | Sonia Gea Ruiz             |
    +----+----------------------------+
    ```
    ---

### Consultas multitabla (Composición externa)

Resuelva todas las consultas utilizando las cláusulas LEFT JOIN y RIGHT JOIN.

---

1. Devuelve un listado con los nombres de todos los profesores y los departamentos que tienen vinculados. El listado también debe mostrar aquellos profesores que no tienen ningún departamento asociado. El listado debe devolver cuatro columnas, nombre del departamento, primer apellido, segundo apellido y nombre del profesor. El resultado estará ordenado alfabéticamente de menor a mayor por el nombre del departamento, apellidos y el nombre.
    ```sql
    SELECT p.apellido1, p.apellido2, p.nombre, d.nombre AS nombre_departamento  
    FROM persona p
    JOIN profesor pf ON p.id = pf.id_profesor 
    LEFT JOIN departamento d ON pf.id_departamento = d.id;

    +------------+------------+-----------+---------------------+
    | apellido1  | apellido2  | nombre    | nombre_departamento |
    +------------+------------+-----------+---------------------+
    | Ramirez    | Gea        | Zoe       | Informática         |
    | Hamill     | Kozey      | Manolo    | Informática         |
    | Schmidt    | Fisher     | David     | Matemáticas         |
    | Kohler     | Schoen     | Alejandro | Matemáticas         |
    | Lemke      | Rutherford | Cristina  | Economía y Empresa  |
    | Fahey      | Considine  | Antonio   | Economía y Empresa  |
    | Spencer    | Lakin      | Esther    | Educación           |
    | Streich    | Hirthe     | Carmen    | Educación           |
    | Ruecker    | Upton      | Guillermo | Educación           |
    | Monahan    | Murray     | Micaela   | Agronomía           |
    | Stiedemann | Morissette | Alfredo   | Química y Física    |
    | Schowalter | Muller     | Francesca | Química y Física    |
    +------------+------------+-----------+---------------------+
    ```
    ---
    
2. Devuelve un listado con los profesores que no están asociados a un departamento.
    ```sql
    SELECT p.apellido1, p.apellido2, p.nombre 
    FROM persona p
    JOIN profesor pf ON p.id = pf.id_profesor 
    LEFT JOIN departamento d ON pf.id_departamento = d.id
    WHERE d.id IS NULL;

    Empty set (0,00 sec)
    ```
    ---
    
3. Devuelve un listado con los departamentos que no tienen profesores asociados.
    ```sql
    SELECT d.nombre
    FROM departamento d
    LEFT JOIN profesor p ON d.id = p.id_departamento
    WHERE p.id_profesor IS NULL;

    +-----------------------+
    | nombre                |
    +-----------------------+
    | Filología             |
    | Derecho               |
    | Biología y Geología   |
    +-----------------------+
    ```
    ---
    
4. Devuelve un listado con los profesores que no imparten ninguna asignatura.
    ```sql
    SELECT pf.id_profesor, CONCAT(p.nombre, ' ', p.apellido1, ' ', p.apellido2) AS nombre_profesor
    FROM profesor pf
    JOIN persona p ON pf.id_profesor = p.id
    LEFT JOIN asignatura a ON pf.id_profesor = a.id_profesor
    WHERE a.id IS NULL;

    +-------------+-------------------------------+
    | id_profesor | nombre_profesor               |
    +-------------+-------------------------------+
    |           5 | David Schmidt Fisher          |
    |          15 | Alejandro Kohler Schoen       |
    |           8 | Cristina Lemke Rutherford     |
    |          16 | Antonio Fahey Considine       |
    |          10 | Esther Spencer Lakin          |
    |          12 | Carmen Streich Hirthe         |
    |          17 | Guillermo Ruecker Upton       |
    |          18 | Micaela Monahan Murray        |
    |          13 | Alfredo Stiedemann Morissette |
    |          20 | Francesca Schowalter Muller   |
    +-------------+-------------------------------+
    ```
    ---
    
5. Devuelve un listado con las asignaturas que no tienen un profesor asignado.
    ```sql
    SELECT a.id, a.nombre
    FROM profesor pf
    RIGHT JOIN asignatura a ON pf.id_profesor = a.id_profesor
    WHERE pf.id_profesor IS NULL;

    +----+---------------------------------------------------------------------------+
    | id | nombre                                                                    |
    +----+---------------------------------------------------------------------------+
    | 22 | Ingeniería de Requisitos                                                  |
    | 23 | Integración de las Tecnologías de la Información en las Organizaciones    |
    | 24 | Modelado y Diseño del Software 1                                          |
    | 25 | Multiprocesadores                                                         |
    | 26 | Seguridad y cumplimiento normativo                                        |
    | 27 | Sistema de Información para las Organizaciones                            |
    | 28 | Tecnologías web                                                           |
    | 29 | Teoría de códigos y criptografía                                          |
    | 30 | Administración de bases de datos                                          |
    | 31 | Herramientas y Métodos de Ingeniería del Software                         |
    | 32 | Informática industrial y robótica                                         |
    | 33 | Ingeniería de Sistemas de Información                                     |
    | 34 | Modelado y Diseño del Software 2                                          |
    | 35 | Negocio Electrónico                                                       |
    | 36 | Periféricos e interfaces                                                  |
    | 37 | Sistemas de tiempo real                                                   |
    | 38 | Tecnologías de acceso a red                                               |
    | 39 | Tratamiento digital de imágenes                                           |
    | 40 | Administración de redes y sistemas operativos                             |
    | 41 | Almacenes de Datos                                                        |
    | 42 | Fiabilidad y Gestión de Riesgos                                           |
    | 43 | Líneas de Productos Software                                              |
    | 44 | Procesos de Ingeniería del Software 1                                     |
    | 45 | Tecnologías multimedia                                                    |
    | 46 | Análisis y planificación de las TI                                        |
    | 47 | Desarrollo Rápido de Aplicaciones                                         |
    | 48 | Gestión de la Calidad y de la Innovación Tecnológica                      |
    | 49 | Inteligencia del Negocio                                                  |
    | 50 | Procesos de Ingeniería del Software 2                                     |
    | 51 | Seguridad Informática                                                     |
    | 52 | Biologia celular                                                          |
    | 53 | Física                                                                    |
    | 54 | Matemáticas I                                                             |
    | 55 | Química general                                                           |
    | 56 | Química orgánica                                                          |
    | 57 | Biología vegetal y animal                                                 |
    | 58 | Bioquímica                                                                |
    | 59 | Genética                                                                  |
    | 60 | Matemáticas II                                                            |
    | 61 | Microbiología                                                             |
    | 62 | Botánica agrícola                                                         |
    | 63 | Fisiología vegetal                                                        |
    | 64 | Genética molecular                                                        |
    | 65 | Ingeniería bioquímica                                                     |
    | 66 | Termodinámica y cinética química aplicada                                 |
    | 67 | Biorreactores                                                             |
    | 68 | Biotecnología microbiana                                                  |
    | 69 | Ingeniería genética                                                       |
    | 70 | Inmunología                                                               |
    | 71 | Virología                                                                 |
    | 72 | Bases moleculares del desarrollo vegetal                                  |
    | 73 | Fisiología animal                                                         |
    | 74 | Metabolismo y biosíntesis de biomoléculas                                 |
    | 75 | Operaciones de separación                                                 |
    | 76 | Patología molecular de plantas                                            |
    | 77 | Técnicas instrumentales básicas                                           |
    | 78 | Bioinformática                                                            |
    | 79 | Biotecnología de los productos hortofrutículas                            |
    | 80 | Biotecnología vegetal                                                     |
    | 81 | Genómica y proteómica                                                     |
    | 82 | Procesos biotecnológicos                                                  |
    | 83 | Técnicas instrumentales avanzadas                                         |
    +----+---------------------------------------------------------------------------+
    ```
    ---
    
6. Devuelve un listado con todos los departamentos que tienen alguna asignatura que no se haya impartido en ningún curso escolar. El resultado debe mostrar el nombre del departamento y el nombre de la asignatura que no se haya impartido nunca.
    ```sql
    SELECT d.nombre AS nombre_departamento, a.nombre AS nombre_asignatura
    FROM departamento d
    LEFT JOIN profesor pf ON d.id = pf.id_departamento
    RIGHT JOIN asignatura a ON pf.id_profesor = a.id_profesor
    LEFT JOIN alumno_se_matricula_asignatura aa ON a.id = aa.id_asignatura
    LEFT JOIN curso_escolar c ON aa.id_curso_escolar = c.id
    WHERE c.id IS NULL;

    +---------------------+---------------------------------------------------------------------------+
    | nombre_departamento | nombre_asignatura                                                         |
    +---------------------+---------------------------------------------------------------------------+
    | Informática         | Arquitectura de Computadores                                              |
    | Informática         | Estructura de Datos y Algoritmos I                                        |
    | Informática         | Ingeniería del Software                                                   |
    | Informática         | Sistemas Inteligentes                                                     |
    | Informática         | Sistemas Operativos                                                       |
    | Informática         | Bases de Datos                                                            |
    | Informática         | Estructura de Datos y Algoritmos II                                       |
    | Informática         | Fundamentos de Redes de Computadores                                      |
    | Informática         | Planificación y Gestión de Proyectos Informáticos                         |
    | Informática         | Programación de Servicios Software                                        |
    | Informática         | Desarrollo de interfaces de usuario                                       |
    | NULL                | Ingeniería de Requisitos                                                  |
    | NULL                | Integración de las Tecnologías de la Información en las Organizaciones    |
    | NULL                | Modelado y Diseño del Software 1                                          |
    | NULL                | Multiprocesadores                                                         |
    | NULL                | Seguridad y cumplimiento normativo                                        |
    | NULL                | Sistema de Información para las Organizaciones                            |
    | NULL                | Tecnologías web                                                           |
    | NULL                | Teoría de códigos y criptografía                                          |
    | NULL                | Administración de bases de datos                                          |
    | NULL                | Herramientas y Métodos de Ingeniería del Software                         |
    | NULL                | Informática industrial y robótica                                         |
    | NULL                | Ingeniería de Sistemas de Información                                     |
    | NULL                | Modelado y Diseño del Software 2                                          |
    | NULL                | Negocio Electrónico                                                       |
    | NULL                | Periféricos e interfaces                                                  |
    | NULL                | Sistemas de tiempo real                                              |
    | NULL                | Tecnologías de acceso a red                                               |
    | NULL                | Tratamiento digital de imágenes                                           |
    | NULL                | Administración de redes y sistemas operativos                             |
    | NULL                | Almacenes de Datos                                                        |
    | NULL                | Fiabilidad y Gestión de Riesgos                                           |
    | NULL                | Líneas de Productos Software                                              |
    | NULL                | Procesos de Ingeniería del Software 1                                     |
    | NULL                | Tecnologías multimedia                                                    |
    | NULL                | Análisis y planificación de las TI                                        |
    | NULL                | Desarrollo Rápido de Aplicaciones                                         |
    | NULL                | Gestión de la Calidad y de la Innovación Tecnológica                      |
    | NULL                | Inteligencia del Negocio                                                  |
    | NULL                | Procesos de Ingeniería del Software 2                                     |
    | NULL                | Seguridad Informática                                                     |
    | NULL                | Biologia celular                                                          |
    | NULL                | Física                                                                    |
    | NULL                | Matemáticas I                                                             |
    | NULL                | Química general                                                           |
    | NULL                | Química orgánica                                                          |
    | NULL                | Biología vegetal y animal                                                 |
    | NULL                | Bioquímica                                                                |
    | NULL                | Genética                                                                  |
    | NULL                | Matemáticas II                                                            |
    | NULL                | Microbiología                                                             |
    | NULL                | Botánica agrícola                                                         |
    | NULL                | Fisiología vegetal                                                        |
    | NULL                | Genética molecular                                                        |
    | NULL                | Ingeniería bioquímica                                                     |
    | NULL                | Termodinámica y cinética química aplicada                                 |
    | NULL                | Biorreactores                                                             |
    | NULL                | Biotecnología microbiana                                                  |
    | NULL                | Ingeniería genética                                                       |
    | NULL                | Inmunología                                                               |
    | NULL                | Virología                                                                 |
    | NULL                | Bases moleculares del desarrollo vegetal                                  |
    | NULL                | Fisiología animal                                                         |
    | NULL                | Metabolismo y biosíntesis de biomoléculas                                 |
    | NULL                | Operaciones de separación                                                 |
    | NULL                | Patología molecular de plantas                                            |
    | NULL                | Técnicas instrumentales básicas                                           |
    | NULL                | Bioinformática                                                            |
    | NULL                | Biotecnología de los productos hortofrutículas                            |
    | NULL                | Biotecnología vegetal                                                     |
    | NULL                | Genómica y proteómica                                                     |
    | NULL                | Procesos biotecnológicos                                                  |
    | NULL                | Técnicas instrumentales avanzadas                                         |
    +---------------------+---------------------------------------------------------------------------+
    ```
    ---
    
### Consultas resumen
Devuelve el número total de alumnas que hay.

---

1. Calcula cuántos alumnos nacieron en 1999.
    ```sql

    SELECT id, CONCAT(nombre, ' ', apellido1, ' ', apellido2) AS nombre_persona, fecha_nacimiento 
    FROM persona
    WHERE YEAR(fecha_nacimiento) = 1999;

    +----+-----------------------------+------------------+
    | id | nombre_persona              | fecha_nacimiento |
    +----+-----------------------------+------------------+
    |  7 | Ismael Strosin Turcotte     | 1999-05-24       |
    | 22 | Antonio Domínguez Guerrero  | 1999-02-11       |
    +----+-----------------------------+------------------+
    ```
    ---
    
2. Calcula cuántos profesores hay en cada departamento. El resultado sólo debe mostrar dos columnas, una con el nombre del departamento y otra con el número de profesores que hay en ese departamento. El resultado sólo debe incluir los departamentos que tienen profesores asociados y deberá estar ordenado de mayor a menor por el número de profesores.
    ```sql
    SELECT d.nombre AS nombre_departamento, COUNT(pf.id_departamento) AS cantidad_profesores
    FROM departamento d
    JOIN profesor pf ON d.id = pf.id_departamento
    GROUP BY d.nombre
    ORDER BY cantidad_profesores DESC;

    +---------------------+---------------------+
    | nombre_departamento | cantidad_profesores |
    +---------------------+---------------------+
    | Educación           |                   3 |
    | Informática         |                   2 |
    | Matemáticas         |                   2 |
    | Economía y Empresa  |                   2 |
    | Química y Física    |                   2 |
    | Agronomía           |                   1 |
    +---------------------+---------------------+

    ```
    ---
    
3. Devuelve un listado con todos los departamentos y el número de profesores que hay en cada uno de ellos. Tenga en cuenta que pueden existir departamentos que no tienen profesores asociados. Estos departamentos también tienen que aparecer en el listado.
    ```sql
    SELECT d.nombre AS nombre_departamento, COUNT(pf.id_departamento) AS cantidad_profesores
    FROM departamento d
    LEFT JOIN profesor pf ON d.id = pf.id_departamento
    GROUP BY d.nombre
    ORDER BY cantidad_profesores DESC;

    +-----------------------+---------------------+
    | nombre_departamento   | cantidad_profesores |
    +-----------------------+---------------------+
    | Educación             |                   3 |
    | Informática           |                   2 |
    | Matemáticas           |                   2 |
    | Economía y Empresa    |                   2 |
    | Química y Física      |                   2 |
    | Agronomía             |                   1 |
    | Filología             |                   0 |
    | Derecho               |                   0 |
    | Biología y Geología   |                   0 |
    +-----------------------+---------------------+
    ```
    ---
    
4. Devuelve un listado con el nombre de todos los grados existentes en la base de datos y el número de asignaturas que tiene cada uno. Tenga en cuenta que pueden existir grados que no tienen asignaturas asociadas. Estos grados también tienen que aparecer en el listado. El resultado deberá estar ordenado de mayor a menor por el número de asignaturas.
    ```sql
    SELECT g.nombre AS nombre_grado, COUNT(a.id) AS cantidad_asignaturas
    FROM grado g
    LEFT JOIN asignatura a ON g.id = a.id_grado
    GROUP BY g.nombre
    ORDER BY cantidad_asignaturas DESC;

    +----------------------------------------------------------+----------------------+
    | nombre_grado                                             | cantidad_asignaturas |
    +----------------------------------------------------------+----------------------+
    | Grado en Ingeniería Informática (Plan 2015)              |                   51 |
    | Grado en Biotecnología (Plan 2015)                       |                   32 |
    | Grado en Ingeniería Agrícola (Plan 2015)                 |                    0 |
    | Grado en Ingeniería Eléctrica (Plan 2014)                |                    0 |
    | Grado en Ingeniería Electrónica Industrial (Plan 2010)   |                    0 |
    | Grado en Ingeniería Mecánica (Plan 2010)                 |                    0 |
    | Grado en Ingeniería Química Industrial (Plan 2010)       |                    0 |
    | Grado en Ciencias Ambientales (Plan 2009)                |                    0 |
    | Grado en Matemáticas (Plan 2010)                         |                    0 |
    | Grado en Química (Plan 2009)                             |                    0 |
    +----------------------------------------------------------+----------------------+
    ```
    ---
    
5. Devuelve un listado con el nombre de todos los grados existentes en la base de datos y el número de asignaturas que tiene cada uno, de los grados que tengan más de 40 asignaturas asociadas.
    ```sql
    SELECT g.nombre AS nombre_grado, COUNT(a.id) AS cantidad_asignaturas
    FROM grado g
    LEFT JOIN asignatura a ON g.id = a.id_grado
    GROUP BY g.nombre
    HAVING cantidad_asignaturas > 40
    ORDER BY cantidad_asignaturas DESC;

    +-----------------------------------------------+----------------------+
    | nombre_grado                                  | cantidad_asignaturas |
    +-----------------------------------------------+----------------------+
    | Grado en Ingeniería Informática (Plan 2015)   |                   51 |
    +-----------------------------------------------+----------------------+
    ```
    ---
    
6. Devuelve un listado que muestre el nombre de los grados y la suma del número total de créditos que hay para cada tipo de asignatura. El resultado debe tener tres columnas: nombre del grado, tipo de asignatura y la suma de los créditos de todas las asignaturas que hay de ese tipo. Ordene el resultado de mayor a menor por el número total de crédidos.
    ```sql
    SELECT g.nombre AS nombre_grado, a.tipo AS tipo_asignatura, SUM(a.creditos) AS cantidad_creditos
    FROM grado g
    LEFT JOIN asignatura a ON g.id = a.id_grado
    GROUP BY g.nombre, a.tipo
    ORDER BY cantidad_creditos DESC;
    
    +----------------------------------------------------------+-----------------+-------------------+
    | nombre_grado                                             | tipo_asignatura | cantidad_creditos |
    +----------------------------------------------------------+-----------------+-------------------+
    | Grado en Ingeniería Informática (Plan 2015)              | optativa        |               180 |
    | Grado en Biotecnología (Plan 2015)                       | obligatoria     |               120 |
    | Grado en Ingeniería Informática (Plan 2015)              | básica          |                72 |
    | Grado en Biotecnología (Plan 2015)                       | básica          |                60 |
    | Grado en Ingeniería Informática (Plan 2015)              | obligatoria     |                54 |
    | Grado en Ingeniería Agrícola (Plan 2015)                 | NULL            |              NULL |
    | Grado en Ingeniería Eléctrica (Plan 2014)                | NULL            |              NULL |
    | Grado en Ingeniería Electrónica Industrial (Plan 2010)   | NULL            |              NULL |
    | Grado en Ingeniería Mecánica (Plan 2010)                 | NULL            |              NULL |
    | Grado en Ingeniería Química Industrial (Plan 2010)       | NULL            |              NULL |
    | Grado en Ciencias Ambientales (Plan 2009)                | NULL            |              NULL |
    | Grado en Matemáticas (Plan 2010)                         | NULL            |              NULL |
    | Grado en Química (Plan 2009)                             | NULL            |              NULL |
    +----------------------------------------------------------+-----------------+-------------------+
    ```
    ---
    
7. Devuelve un listado que muestre cuántos alumnos se han matriculado de alguna asignatura en cada uno de los cursos escolares. El resultado deberá mostrar dos columnas, una columna con el año de inicio del curso escolar y otra con el número de alumnos matriculados.
    ```sql

    ```
    ---
    
8. Devuelve un listado con el número de asignaturas que imparte cada profesor. El listado debe tener en cuenta aquellos profesores que no imparten ninguna asignatura. El resultado mostrará cinco columnas: id, nombre, primer apellido, segundo apellido y número de asignaturas. El resultado estará ordenado de mayor a menor por el número de asignaturas.
    ```sql

    ```
    ---
    

