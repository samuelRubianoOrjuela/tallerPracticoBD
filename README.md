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

    ```
    ---
    
5. Devuelve un listado con el nombre de todos los departamentos que tienen profesores que imparten alguna asignatura en el Grado en Ingeniería Informática (Plan 2015).
    ```sql

    ```
    ---
    
6. Devuelve un listado con todos los alumnos que se han matriculado en alguna asignatura durante el curso escolar 2018/2019.
    ```sql

    ```
    ---
    