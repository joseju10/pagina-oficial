# Auditoría

1- Activa desde SQL*Plus la auditoría de los intentos de acceso exitosos al sistema. Comprueba su funcionamiento.

Para activar la auditoría de los intentos de acceso exitosos al sistema desde SQL*Plus, se puede utilizar el siguiente comando:

```sql
SQL> AUDIT CREATE SESSION;

Auditoria terminada correctamente.
```

Este comando activará la auditoría de los intentos de acceso exitosos al sistema. Cada vez que un usuario inicie sesión correctamente en la base de datos, se registrará en el archivo de auditoría.

Para comprobar el funcionamiento de la auditoría, se puede iniciar sesión con un usuario existente en la base de datos. En mi caso inicio sesión con el usuario SCOTT, pero primero introduciremos los datos erróneos a drede para que se registren dos entradas de inicio de sesión:

```sql
oracle@oracle:~$ sqlplus SCOTT/TIGER

SQL*Plus: Release 19.0.0.0.0 - Production on Mon Feb 20 08:48:44 2023
Version 19.3.0.0.0

Copyright (c) 1982, 2019, Oracle.  All rights reserved.

ERROR:
ORA-01017: nombre de usuario/contrase?a no validos; conexion denegada


Enter user-name: SCOTT
Enter password: 
Hora de Ultima Conexion Correcta: Lun Feb 13 2023 14:01:40 +01:00

Conectado a:
Oracle Database 19c Enterprise Edition Release 19.0.0.0.0 - Production
Version 19.3.0.0.0

SQL> 
```

Si la auditoría está funcionando correctamente, se debe registrar un evento en el archivo de auditoría. Para ver los eventos de auditoría registrados, se puede ejecutar el siguiente comando:

```sql
SQL> SELECT OS_USERNAME, USERNAME, EXTENDED_TIMESTAMP, ACTION_NAME FROM DBA_AUDIT_SESSION;

OS_USERNAME
--------------------------------------------------------------------------------
USERNAME
--------------------------------------------------------------------------------
EXTENDED_TIMESTAMP
---------------------------------------------------------------------------
ACTION_NAME
----------------------------
oracle
SCOTT
20/02/23 08:48:45,439076 +01:00
LOGON


OS_USERNAME
--------------------------------------------------------------------------------
USERNAME
--------------------------------------------------------------------------------
EXTENDED_TIMESTAMP
---------------------------------------------------------------------------
ACTION_NAME
----------------------------
oracle
SCOTT
20/02/23 08:48:51,086682 +01:00
LOGON
```

Este comando mostrará todos los eventos de auditoría registrados en la base de datos. Si los eventos de inicio de sesión están presentes en la lista, significa que la auditoría está funcionando correctamente.

2- Realiza un procedimiento en PL/SQL que te muestre los accesos fallidos junto con el motivo de los mismos, transformando el código de error almacenado en un mensaje de texto comprensible. Contempla todos los motivos posibles para que un acceso sea fallido.

Primero realizamos el procedimiento y lo compilamos:

```sql
SQL> CREATE OR REPLACE PROCEDURE mostrar_accesos_fallidos AS
  2    cursor c_datos is SELECT * FROM dba_audit_trail WHERE action_name = 'LOGON' AND returncode > 0;
  3    v_return VARCHAR2(1000);
  4  BEGIN
  5    FOR rec IN c_datos LOOP
  6      v_return := 'Intento fallido de inicio de sesión. ';
  7      CASE rec.returncode
  8         WHEN 1017 THEN v_return := v_return || 'Contraseña incorrecta.';
  9         WHEN 28000 THEN v_return := v_return || 'Cuenta bloqueada.';
  10        WHEN 1012 THEN v_return := v_return || 'Usuario no existe.';
  11        WHEN 1005 THEN v_return := v_return || 'Contraseña expirada.';
  12        WHEN 1090 THEN v_return := v_return || 'Perfil de usuario no permitido.';
  13        ELSE v_return := v_return || 'Código de error: ' || rec.returncode;
  14      END CASE;
  15      DBMS_OUTPUT.PUT_LINE(v_return || ' Usuario: ' || rec.username || ' Cliente: ' || rec.client_id || ' Fecha: ' || rec.EXTENDED_TIMESTAMP);
  16    END LOOP;
  17  END;
  18  /

PROCEDURE created.

Commit complete.
```

Finalmente lo ejecutamos y comprobamos que funciona:

```sql
SQL> exec mostrar_accesos_fallidos;

Intento fallido de inicio de sesión. Contraseña incorrecta. Usuario: SCOTT
Cliente:  Fecha: 20-FEB-23 08.48.45.439076 AM EUROPE/MADRID

PL/SQL procedure successfully completed.

Commit complete.
```

3- Activa la auditoría de las operaciones DML realizadas por SCOTT. Comprueba su funcionamiento.

Para activar la auditoría de las operaciones DML (Data Manipulation Language) en Oracle para el usuario SCOTT, se puede seguir los siguientes pasos:

Activamos la auditoría de las operaciones DML para el usuario SCOTT:

```sql
SQL> AUDIT INSERT TABLE, UPDATE TABLE, DELETE TABLE ON SCOTT.* BY ACCESS;

Auditoria terminada correctamente.
```

Realizamos un par de operaciones DML con el usuario SCOTT, por ejemplo, elimnar e insertar una fila en la tabla EMP:

sql

```sql
SQL> DELETE FROM EMP WHERE EMPNO = 9999;

1 fila suprimida.

SQL> INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
VALUES (9999, 'JUAN', 'MANAGER', 7839, SYSDATE, 5000, NULL, 10); 

1 fila creada.
```

Comprobamos las operaciones realizadas se han registrado:

```sql
SQL> SELECT USERNAME, OBJ_NAME, ACTION_NAME, EXTENDED_TIMESTAMP FROM DBA_AUDIT_OBJECT;

USERNAME
--------------------------------------------------------------------------------
OBJ_NAME
--------------------------------------------------------------------------------
ACTION_NAME
----------------------------
EXTENDED_TIMESTAMP
---------------------------------------------------------------------------
SCOTT
EMP
DELETE
20/02/23 13:48:32,479946 +01:00


USERNAME
--------------------------------------------------------------------------------
OBJ_NAME
--------------------------------------------------------------------------------
ACTION_NAME
----------------------------
EXTENDED_TIMESTAMP
---------------------------------------------------------------------------
SCOTT
EMP
INSERT
20/02/23 13:55:49,759488 +01:00
```

4- Realiza una auditoría de grano fino para almacenar información sobre la inserción de empleados con sueldo superior a 2000 en la tabla emp de scott.

Activamos la auditoría para la tabla EMP utilizando la siguiente sentencia SQL:

```sql
SQL> BEGIN
	DBMS_FGA.ADD_POLICY (
	OBJECT_SCHEMA      => 'SCOTT',
	OBJECT_NAME        => 'EMP',
	POLICY_NAME        => 'AUD_FINA_JOSEJU',
	AUDIT_CONDITION    => 'SAL>2000',
	STATEMENT_TYPES    => 'INSERT, UPDATE'
	);
END;
/  2    3    4    5    6    7    8    9   10  

Procedimiento PL/SQL terminado correctamente.
```

Realizamos varios inserts y un update en la tabla EMP que cumpla con la condición de auditoría para comprobar que se está registrando la información de auditoría:

```sql
SQL> INSERT INTO SCOTT.EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
VALUES (8003, 'JONES', 'ANALYST', 7839, DATE '2023-01-01', 4000, NULL, 20);
1 fila creada.

SQL> INSERT INTO SCOTT.EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) 
VALUES (8002, 'MARTIN', 'SALESMAN', 7698, DATE '2023-01-01', 2500, 500, 30);

1 fila creada.

SQL> INSERT INTO SCOTT.EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) 
VALUES (8001, 'JOHNSON', 'MANAGER', 7839, DATE '2023-01-01', 5000, NULL, 10);  2  

1 fila creada.

SQL> UPDATE SCOTT.EMP 
SET SAL = 3000 
WHERE EMPNO = 7499;  2    3  

1 fila actualizada.
```

Si intentamos hacer un insert con un salario menor que 2000, no se añadira:

```sql
SQL> INSERT INTO SCOTT.EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
VALUES (8004, 'SMITH', 'CLERK', 7902, SYSDATE, 1000, NULL, 20);
  2  
1 fila creada.
```

Consulta la vista DBA_AUDIT_TRAIL para verificar que se haya registrado la información de auditoría para la inserción de empleados con salario superior a 2000:

```sql
SQL> SELECT DB_USER, OBJECT_NAME, SQL_TEXT, EXTENDED_TIMESTAMP FROM DBA_FGA_AUDIT_TRAIL WHERE POLICY_NAME='AUD_FINA_JOSEJU';

DB_USER
--------------------------------------------------------------------------------
OBJECT_NAME
--------------------------------------------------------------------------------
SQL_TEXT
--------------------------------------------------------------------------------
EXTENDED_TIMESTAMP
---------------------------------------------------------------------------
SCOTT
EMP
INSERT INTO SCOTT.EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
VALUES (8003, 'JONES', 'ANALYST', 7839, DATE '2023-01-01', 4000, NULL, 20)
20/02/23 15:24:31,191182 +01:00

DB_USER
--------------------------------------------------------------------------------
OBJECT_NAME
--------------------------------------------------------------------------------
SQL_TEXT
--------------------------------------------------------------------------------
EXTENDED_TIMESTAMP
---------------------------------------------------------------------------

SCOTT
EMP
INSERT INTO SCOTT.EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
VALUES (8002, 'MARTIN', 'SALESMAN', 7698, DATE '2023-01-01', 2500, 500, 30)

DB_USER
--------------------------------------------------------------------------------
OBJECT_NAME
--------------------------------------------------------------------------------
SQL_TEXT
--------------------------------------------------------------------------------
EXTENDED_TIMESTAMP
---------------------------------------------------------------------------
20/02/23 15:25:07,918755 +01:00

SCOTT
EMP
INSERT INTO SCOTT.EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)

DB_USER
--------------------------------------------------------------------------------
OBJECT_NAME
--------------------------------------------------------------------------------
SQL_TEXT
--------------------------------------------------------------------------------
EXTENDED_TIMESTAMP
---------------------------------------------------------------------------
VALUES (8001, 'JOHNSON', 'MANAGER', 7839, DATE '2023-01-01', 5000, NULL, 10)
20/02/23 15:25:53,252817 +01:00

SCOTT
EMP

DB_USER
--------------------------------------------------------------------------------
OBJECT_NAME
--------------------------------------------------------------------------------
SQL_TEXT
--------------------------------------------------------------------------------
EXTENDED_TIMESTAMP
---------------------------------------------------------------------------
UPDATE SCOTT.EMP
SET SAL = 3000
WHERE EMPNO = 7499
20/02/23 15:26:03,546799 +01:00
```

También como hemos podido comprobar, no se ha añadido al empleado con id '8004' debido a que su salario es inferior a 2000.

5- Explica la diferencia entre auditar una operación by access o by session ilustrándolo con ejemplos.

En Oracle, es posible auditar una operación en dos niveles: by access y by session.

La auditoría "by access" registra información de auditoría para cada intento de acceso a un objeto, independientemente de si se realiza una operación en el objeto o no. 

Por ejemplo, si se configura la auditoría by access en una tabla, cada vez que se intente acceder a la tabla, se registrará información de auditoría, como el usuario que intentó acceder, el momento en que se realizó el intento de acceso, etc. La auditoría by access es útil para monitorear los intentos de acceso no autorizados a un objeto.

Por otro lado, la auditoría "by session" registra información de auditoría para todas las operaciones realizadas en un objeto durante una sesión de usuario. 

Por ejemplo, si se configura la auditoría by session en una tabla, se registrará información de auditoría para todas las operaciones realizadas en la tabla durante la sesión de usuario, incluyendo los select, insert, update y delete. La auditoría by session es útil para monitorear las operaciones realizadas por un usuario en particular durante una sesión.

En resumen, la principal diferencia entre la auditoría by access y by session es el nivel de detalle de la información de auditoría que se registra. La auditoría by access registra información de auditoría para cada intento de acceso a un objeto, mientras que la auditoría by session registra información de auditoría para todas las operaciones realizadas en un objeto durante una sesión de usuario.

Ejemplo:

```sql
SQL> AUDIT INSERT TABLE, UPDATE TABLE, DELETE TABLE ON SCOTT.* BY ACCESS/SESSION;
```

6- Documenta las diferencias entre los valores db y db, extended del parámetro audit_trail de ORACLE. Demuéstralas poniendo un ejemplo de la información sobre una operación concreta recopilada con cada uno de ellos.

El parámetro audit_trail de Oracle se utiliza para especificar dónde se guardará la información de auditoría. Hay dos valores posibles para este parámetro: db y db, extended.

La opción db registra la información de auditoría en la tabla de auditoría SYS.AUD$. Esta opción almacena información como la hora de la operación, el usuario que realizó la operación, el objeto en el que se realizó la operación y el tipo de operación realizada. Esta opción es más adecuada para auditorías básicas de seguridad.

Por otro lado, la opción db, extended almacena información más detallada sobre la operación de auditoría, incluyendo el comando SQL completo que se ejecutó, la dirección IP del usuario, el programa que se utilizó para realizar la operación, entre otros detalles. Esta opción es más adecuada para auditorías avanzadas que requieren información más detallada sobre las operaciones realizadas.

Como ejemplo de funcionamiento haremos lo siguiente:

- Primero mostraremos que parámetro tiene audit_trail:

```sql
SQL> SHOW PARAMETER AUDIT_TRAIL;

NAME				                         TYPE	       VALUE
------------------------------------ ----------- ------------------------------
audit_trail			                     string	     DB
```

Procederemos a cambiar el valor de nuestra base de datos de “db” a “db, extended” ejecutando lo siguiente:

```sql
SQL> ALTER SYSTEM SET audit_trail = DB,EXTENDED SCOPE=SPFILE;

Sistema modificado.

SQL> SHUTDOWN
Base de datos cerrada.
Base de datos desmontada.
Instancia ORACLE cerrada.
SQL> STARTUP
Instancia ORACLE iniciada.

Total System Global Area  830470160 bytes
Fixed Size		    9140240 bytes
Variable Size		  574619648 bytes
Database Buffers	  243269632 bytes
Redo Buffers		    3440640 bytes
Base de datos montada.
Base de datos abierta.

SQL> SHOW PARAMETER AUDIT_TRAIL;

NAME				                         TYPE	       VALUE
------------------------------------ ----------- ------------------------------
audit_trail			                     string	     DB, EXTENDED
```

7- Averigua si en Postgres se pueden realizar los cuatro primeros apartados. Si es así, documenta el proceso adecuadamente.

Activa desde Postgres la auditoría de los intentos de acceso exitosos al sistema:

- Es tan sencillo como consultar el documento /var/log/postgresql/postgresql-13-main.log:

```shell
postgres@postgres1:~$ cat /var/log/postgresql/postgresql-13-main.log

2023-02-22 01:02:55.843 CET [50146] scott@scott LOG:  el nombre de usuario entregado (scott) y el nombre de usuario autentificado (postgres) no coinciden
2023-02-22 01:02:55.844 CET [50146] scott@scott FATAL:  la autentificación Peer falló para el usuario «scott»
2023-02-22 01:02:55.844 CET [50146] scott@scott DETALLE:  La conexión coincidió con la línea 94 de pg_hba.conf: «local   all             all                                     peer»
2023-02-22 01:03:05.950 CET [50158] scott@scott LOG:  el nombre de usuario entregado (scott) y el nombre de usuario autentificado (postgres) no coinciden
2023-02-22 01:03:05.950 CET [50158] scott@scott FATAL:  la autentificación Peer falló para el usuario «scott»
2023-02-22 01:03:05.950 CET [50158] scott@scott DETALLE:  La conexión coincidió con la línea 94 de pg_hba.conf: «local   all             all                                     peer»
2023-02-22 01:03:54.055 CET [50203] scott@scott LOG:  el nombre de usuario entregado (scott) y el nombre de usuario autentificado (postgres) no coinciden
2023-02-22 01:03:54.055 CET [50203] scott@scott FATAL:  la autentificación Peer falló para el usuario «scott»
2023-02-22 01:03:54.055 CET [50203] scott@scott DETALLE:  La conexión coincidió con la línea 94 de pg_hba.conf: «local   all             all                                     peer»
```

Realiza un procedimiento en plpgsql que te muestre los accesos fallidos junto con el motivo de los mismos, transformando el código de error almacenado en un mensaje de texto comprensible. Contempla todos los motivos posibles para que un acceso sea fallido.

- Compilamos los códigos:

```sql
postgres=# CREATE OR REPLACE PROCEDURE obtener_actividades_fallidas() LANGUAGE plpgsql AS $$
DECLARE
    actividad pg_stat_activity%ROWTYPE;
BEGIN
    FOR actividad IN SELECT * FROM pg_stat_activity WHERE state = 'failed' LOOP
        PERFORM mostrar_motivo_error(actividad);
    END LOOP;
END;
$$;
CREATE PROCEDURE

postgres=# CREATE TYPE actividad_registro AS (
  datid oid,
  datname name,
  pid integer,
  usename name,
  application_name text,
  client_addr inet,
  client_hostname text,
  backend_start timestamp with time zone,
  state_change timestamp with time zone,
  state text,
  query text
);
CREATE TYPE

postgres=# CREATE OR REPLACE PROCEDURE mostrar_motivo_error(actividad actividad_registro) LANGUAGE plpgsql AS $$
DECLARE
    error_msg TEXT;
BEGIN
    error_msg := pg_last_error(actividad.pid);
    CASE
        WHEN error_msg ILIKE '%password authentication failed%' THEN
            RAISE NOTICE 'El usuario % intentó conectarse y falló debido a una contraseña incorrecta.', actividad.usename;
        WHEN error_msg ILIKE '%authentication failed%' THEN
            RAISE NOTICE 'El usuario % intentó conectarse y falló debido a un error de autenticación.', actividad.usename;
        WHEN error_msg ILIKE '%account has expired%' THEN
            RAISE NOTICE 'El usuario % intentó conectarse y falló porque la cuenta ha expirado.', actividad.usename;
        WHEN error_msg ILIKE '%account is locked%' THEN
            RAISE NOTICE 'El usuario % intentó conectarse y falló porque la cuenta está bloqueada.', actividad.usename;
        WHEN error_msg ILIKE '%password expired%' THEN
            RAISE NOTICE 'El usuario % intentó conectarse y falló porque la contraseña ha expirado.', actividad.usename;
        WHEN error_msg ILIKE '%password required%' THEN
            RAISE NOTICE 'El usuario % intentó conectarse y falló porque se requiere una contraseña.', actividad.usename;
        WHEN error_msg ILIKE '%password reuse refused%' THEN
            RAISE NOTICE 'El usuario % intentó conectarse y falló porque la contraseña está en el historial de contraseñas.', actividad.usename;
        ELSE
            RAISE NOTICE 'El usuario % intentó conectarse y falló por una razón desconocida. Mensaje de error: %', actividad.usename, error_msg;
    END CASE;
END;
$$;
CREATE PROCEDURE

postgres=# CREATE OR REPLACE PROCEDURE mostrar_accesos_fallidos() LANGUAGE plpgsql AS $$
BEGIN
    PERFORM obtener_actividades_fallidas();
END;
$$;
CREATE PROCEDURE
```

- Intentamos acceder con un usuario:

```sql
postgres=# \connect scott scott
La contraseña es incorrecta
```

- Ejecutamos el procedimiento para ver si se ha realizado correctamente:

```sql
postgres=# CALL mostrar_accesos_fallidos();
NOTICE:  El usuario scott intentó conectarse y falló debido a una contraseña incorrecta.
CALL
```

Activa la auditoría de las operaciones DML realizadas por SCOTT. Comprueba su funcionamiento.

- Descargamos e instalamos la herramienta:

```sql
postgres@postgres1:~/descargas$ wget https://raw.githubusercontent.com/2ndQuadrant/audit-trigger/master/audit.sql
--2023-02-22 00:50:17--  https://raw.githubusercontent.com/2ndQuadrant/audit-trigger/master/audit.sql
Resolviendo raw.githubusercontent.com (raw.githubusercontent.com)... 185.199.109.133, 185.199.108.133, 185.199.110.133, ...
Conectando con raw.githubusercontent.com (raw.githubusercontent.com)[185.199.109.133]:443... conectado.
Petición HTTP enviada, esperando respuesta... 200 OK
Longitud: 11832 (12K) [text/plain]
Grabando a: «audit.sql»

audit.sql                               100%[=============================================================================>]  11,55K  --.-KB/s    en 0s      

2023-02-22 00:50:17 (22,7 MB/s) - «audit.sql» guardado [11832/11832]

postgres@postgres1:~/descargas$ psql
psql (13.8 (Debian 13.8-0+deb11u1))
Digite «help» para obtener ayuda.

postgres=# \i audit.sql
CREATE EXTENSION
CREATE SCHEMA
REVOKE
COMMENT
CREATE TABLE
REVOKE
COMMENT
COMMENT
COMMENT
COMMENT
COMMENT
COMMENT
COMMENT
COMMENT
COMMENT
COMMENT
COMMENT
COMMENT
COMMENT
COMMENT
COMMENT
COMMENT
COMMENT
COMMENT
CREATE INDEX
CREATE INDEX
CREATE INDEX
CREATE FUNCTION
COMMENT
CREATE FUNCTION
COMMENT
CREATE FUNCTION
CREATE FUNCTION
COMMENT
CREATE VIEW
COMMENT
```

- Activamos la auditoria para la tabla emp del esquema scott:

```sql
postgres=# select audit.audit_table('scott.emp');
NOTICE:  disparador «audit_trigger_row» para la relación «scott.emp» no existe, omitiendo
NOTICE:  disparador «audit_trigger_stm» para la relación «scott.emp» no existe, omitiendo
NOTICE:  CREATE TRIGGER audit_trigger_row AFTER INSERT OR UPDATE OR DELETE ON scott.emp FOR EACH ROW EXECUTE PROCEDURE audit.if_modified_func('true');
NOTICE:  CREATE TRIGGER audit_trigger_stm AFTER TRUNCATE ON scott.emp FOR EACH STATEMENT EXECUTE PROCEDURE audit.if_modified_func('true');
 audit_table 
-------------
 
(1 fila)
```

- Realizamos algunas operaciones dml con el usuario 'scott':

```sql
scott=# INSERT INTO scott.emp (empno, ename, job, mgr, hiredate, sal, comm, deptno) 
VALUES (7934, 'MILLER', 'CLERK', 7782, '1982-01-23', 1300, NULL, 10);
INSERT 0 1
scott=# UPDATE scott.emp SET sal = 1500 WHERE empno = 7934;
UPDATE 1
```

- Realizamos una consulta para comprobar que se han registrado todas las operacione dml del usuario scott:

```sql
postgres=# select session_user_name, action, table_name, action_tstamp_clk, client_query from audit.logged_actions;

 session_user_name | action | table_name |       action_tstamp_clk       |                                 client_query                                 
-------------------+--------+------------+-------------------------------+------------------------------------------------------------------------------
 postgres          | I      | emp        | 2023-02-22 01:09:38.281144+01 | INSERT INTO scott.emp (empno, ename, job, mgr, hiredate, sal, comm, deptno) +
                   |        |            |                               | VALUES (7934, 'MILLER', 'CLERK', 7782, '1982-01-23', 1300, NULL, 10);
 postgres          | U      | emp        | 2023-02-22 01:09:43.904374+01 | UPDATE scott.emp SET sal = 1500 WHERE empno = 7934;
(2 filas)
```

Realiza una auditoría de grano fino en postgres para almacenar información sobre la inserción de empleados con sueldo superior a 2000 en la tabla emp de scott.

- Creamos la tabla de auditoría:

```sql
postgres=# CREATE TABLE scott.emp_audit (
  audit_time TIMESTAMP NOT NULL DEFAULT now(),
  user_name TEXT NOT NULL,
  action TEXT NOT NULL,
  empno INTEGER NOT NULL,
  ename TEXT NOT NULL,
  job TEXT NOT NULL,
  mgr INTEGER,
  hiredate DATE NOT NULL,
  sal NUMERIC(8,2) NOT NULL,
  comm NUMERIC(8,2),
  deptno INTEGER NOT NULL
);
CREATE TABLE
```

- Creamos la función que será llamada por el trigger:

```sql
postgres=# CREATE OR REPLACE FUNCTION scott.emp_audit_trigger()
  RETURNS TRIGGER AS $$
BEGIN
  IF NEW.sal > 2000 THEN
    INSERT INTO scott.emp_audit (user_name, action, empno, ename, job, mgr, hiredate, sal, comm, deptno)
      VALUES (current_user, 'INSERT', NEW.empno, NEW.ename, NEW.job, NEW.mgr, NEW.hiredate, NEW.sal, NEW.comm, NEW.deptno);
  END IF;
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;
CREATE FUNCTION
```

Creamos el trigger que llamará a la función en cada inserción:

```sql
postgres=# CREATE TRIGGER emp_insert_audit_trigger
  AFTER INSERT ON scott.emp
  FOR EACH ROW
  EXECUTE FUNCTION scott.emp_audit_trigger();
CREATE TRIGGER
```

Con estos pasos, se creará una tabla de auditoría y un trigger que se ejecutará después de cada inserción en la tabla emp. 

Si el nuevo registro tiene un salario superior a 2000, se registrará la información en la tabla de auditoría. 

Los campos de la tabla de auditoría son audit_time (timestamp), user_name (nombre del usuario que realizó la inserción), action (tipo de acción realizada), empno (número de empleado), ename (nombre del empleado), job (puesto de trabajo), mgr (número del supervisor), hiredate (fecha de contratación), sal (salario), comm (comisión) y deptno (número del departamento).

- A continuación, insertaremos un registro donde el salario es inferior a 2000, con lo que no se insertara nada en la tabla de auditoría:

```sql
postgres=# INSERT INTO scott.emp (empno, ename, job, mgr, hiredate, sal, comm, deptno)
VALUES (7935, 'Miller', 'Clerk', 7782, '1982-01-23', 1300, NULL, 10);
INSERT 0 1
postgres=# select *
postgres-# from scott.emp_audit;
 audit_time | user_name | action | empno | ename | job | mgr | hiredate | sal | comm | deptno 
------------+-----------+--------+-------+-------+-----+-----+----------+-----+------+--------
(0 filas)
```

- Finalmente insertamos un empleado con un salario mayor a 200 y comprobaremo que funciona:

```sql
postgres=# INSERT INTO scott.emp (empno, ename, job, mgr, hiredate, sal, comm, deptno)
VALUES (7902, 'Ford', 'Analyst', 7566, '1981-12-03', 3000, NULL, 20);
INSERT 0 1
postgres=# select *                                                                   
from scott.emp_audit;
         audit_time         | user_name | action | empno | ename |   job   | mgr  |  hiredate  |   sal   | comm | deptno 
----------------------------+-----------+--------+-------+-------+---------+------+------------+---------+------+--------
 2023-02-22 01:19:55.835913 | postgres  | INSERT |  7902 | Ford  | Analyst | 7566 | 1981-12-03 | 3000.00 |      |     20
(1 fila)
```

8- Averigua si en MySQL se pueden realizar los apartados 1, 3 y 4. Si es así, documenta el proceso adecuadamente.

Activa desde SQL*Plus la auditoría de los intentos de acceso exitosos al sistema. Comprueba su funcionamiento.


9- Averigua las posibilidades que ofrece MongoDB para auditar los cambios que va sufriendo un documento. Demuestra su funcionamiento.



10- Averigua si en MongoDB se pueden auditar los accesos a una colección concreta. Demuestra su funcionamiento.


