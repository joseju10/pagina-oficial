# INTERCONEXIÓN DE SERVIDORES

## Introducción

En esta práctica veremos varias formas de crear un enlace entre distintos servidores de bases de datos.

Se pide:

- Realizar un enlace entre dos servidores de bases de datos ORACLE, explicando la configuración necesaria en ambos extremos y demostrando su funcionamiento.
      
- Realizar un enlace entre dos servidores de bases de datos Postgres, explicando la configuración necesaria en ambos extremos y demostrando su funcionamiento.
      
- Realizar un enlace entre un servidor ORACLE y otro Postgres empleando Heterogeneus Services, explicando la configuración necesaria en ambos extremos y demostrando su funcionamiento.

## Interconexión entre dos servidores Oracle.

Lo primero que haremos en este punto es crear dos bases de datos en cada uno de los servidores de oracle y las conectaremos entre sí. Para ello seguiremos los correspondientes pasos:

- Primero creamos un usuario en la maquina oracle1 otorgandole todos los privilegios:

    ```sql
    SQL> CREATE USER joseju10 IDENTIFIED BY joseju10;

    Usuario creado.

    SQL> GRANT ALL PRIVILEGES TO joseju10;

    Concesion terminada correctamente.
    ```

- Posteriormente, nos conectamos al usuario recién creado:

    ```sql
    SQL> DISCONNECT
    Desconectado de Oracle Database 19c Enterprise Edition Release 19.0.0.0.0 - Production
    Version 19.3.0.0.0
    SQL> CONNECT joseju10/joseju10
    Conectado.
    ```

- Crearemos una base de datos de ejemplo, en este caso el esquema SCOTT:

    ```sql
    CREATE TABLE DEPT
    (
    DEPTNO NUMBER(2),
    DNAME VARCHAR2(14),
    LOC VARCHAR2(13),
    CONSTRAINT PK_DEPT PRIMARY KEY (DEPTNO)
    );

    CREATE TABLE EMP
    (
    EMPNO NUMBER(4),
    ENAME VARCHAR2(10),
    JOB VARCHAR2(9),
    MGR NUMBER(4),
    HIREDATE DATE,
    SAL NUMBER(7, 2),
    COMM NUMBER(7, 2),
    DEPTNO NUMBER(2),
    CONSTRAINT FK_DEPTNO FOREIGN KEY (DEPTNO) REFERENCES DEPT (DEPTNO),
    CONSTRAINT PK_EMP PRIMARY KEY (EMPNO)
    );

    INSERT INTO DEPT VALUES (10, 'ACCOUNTING', 'NEW YORK');
    INSERT INTO DEPT VALUES (20, 'RESEARCH', 'DALLAS');
    INSERT INTO DEPT VALUES (30, 'SALES', 'CHICAGO');
    INSERT INTO DEPT VALUES (40, 'OPERATIONS', 'BOSTON');

    INSERT INTO EMP VALUES(7369, 'SMITH', 'CLERK', 7902,TO_DATE('17-DIC-1980', 'DD-MON-YYYY'), 800, NULL, 20);
    INSERT INTO EMP VALUES(7499, 'ALLEN', 'SALESMAN', 7698,TO_DATE('20-FEB-1981', 'DD-MON-YYYY'), 1600, 300, 30);
    INSERT INTO EMP VALUES(7521, 'WARD', 'SALESMAN', 7698,TO_DATE('22-FEB-1981', 'DD-MON-YYYY'), 1250, 500, 30);
    INSERT INTO EMP VALUES(7566, 'JONES', 'MANAGER', 7839,TO_DATE('2-ABR-1981', 'DD-MON-YYYY'), 2975, NULL, 20);
    INSERT INTO EMP VALUES(7654, 'MARTIN', 'SALESMAN', 7698,TO_DATE('28-SEP-1981', 'DD-MON-YYYY'), 1250, 1400, 30);
    INSERT INTO EMP VALUES(7698, 'BLAKE', 'MANAGER', 7839,TO_DATE('1-MAY-1981', 'DD-MON-YYYY'), 2850, NULL, 30);
    INSERT INTO EMP VALUES(7782, 'CLARK', 'MANAGER', 7839,TO_DATE('9-JUN-1981', 'DD-MON-YYYY'), 2450, NULL, 10);
    INSERT INTO EMP VALUES(7788, 'SCOTT', 'ANALYST', 7566,TO_DATE('09-DIC-1982', 'DD-MON-YYYY'), 3000, NULL, 20);
    INSERT INTO EMP VALUES(7839, 'KING', 'PRESIDENT', NULL,TO_DATE('17-NOV-1981', 'DD-MON-YYYY'), 5000, NULL, 10);
    INSERT INTO EMP VALUES(7844, 'TURNER', 'SALESMAN', 7698,TO_DATE('8-SEP-1981', 'DD-MON-YYYY'), 1500, 0, 30);
    INSERT INTO EMP VALUES(7876, 'ADAMS', 'CLERK', 7788,TO_DATE('12-ENE-1983', 'DD-MON-YYYY'), 1100, NULL, 20);
    INSERT INTO EMP VALUES(7900, 'JAMES', 'CLERK', 7698,TO_DATE('3-DIC-1981', 'DD-MON-YYYY'), 950, NULL, 30);
    INSERT INTO EMP VALUES(7902, 'FORD', 'ANALYST', 7566,TO_DATE('3-DIC-1981', 'DD-MON-YYYY'), 3000, NULL, 20);
    INSERT INTO EMP VALUES(7934, 'MILLER', 'CLERK', 7782,TO_DATE('23-ENE-1982', 'DD-MON-YYYY'), 1300, NULL, 10);

    COMMIT;
    ```

Realizamos el mismo procedimiento en oracle2, creando el usuario joseju20, y el esquema SCOTT modificando los inserts:

- Creo usuario:

    ```sql
    SQL> CREATE USER joseju20 IDENTIFIED BY joseju20;

    Usuario creado.

    SQL> GRANT ALL PRIVILEGES TO joseju20;

    Concesion terminada correctamente.
    ```

- Nos conectamos al usuario y creamos la siguiente base de datos:

    ```sql
    CREATE TABLE DEPT
    (
    DEPTNO NUMBER(2),
    DNAME VARCHAR2(14),
    LOC VARCHAR2(13),
    CONSTRAINT PK_DEPT PRIMARY KEY (DEPTNO)
    );

    CREATE TABLE EMP
    (
    EMPNO NUMBER(4),
    ENAME VARCHAR2(10),
    JOB VARCHAR2(9),
    MGR NUMBER(4),
    HIREDATE DATE,
    SAL NUMBER(7, 2),
    COMM NUMBER(7, 2),
    DEPTNO NUMBER(2),
    CONSTRAINT FK_DEPTNO FOREIGN KEY (DEPTNO) REFERENCES DEPT (DEPTNO),
    CONSTRAINT PK_EMP PRIMARY KEY (EMPNO)
    );

    INSERT INTO DEPT VALUES (50, 'RECOUNTING', 'MIAMI');
    INSERT INTO DEPT VALUES (60, 'SEARCH', 'FLORIDA');
    INSERT INTO DEPT VALUES (70, 'SHELLS', 'NEW JERSEY');
    INSERT INTO DEPT VALUES (80, 'OPTIONS', 'WASHINTON');

    INSERT INTO EMP VALUES(7379, 'LEON', 'EMPLOY', 7902,TO_DATE('18-DIC-1980', 'DD-MON-YYYY'), 800, NULL, 60);
    INSERT INTO EMP VALUES(7419, 'NEIL', 'SALESMAN', 7698,TO_DATE('21-FEB-1981', 'DD-MON-YYYY'), 1600, 300, 80);
    INSERT INTO EMP VALUES(7531, 'RICKY', 'SALESMAN', 7698,TO_DATE('23-FEB-1981', 'DD-MON-YYYY'), 1250, 500, 80);
    INSERT INTO EMP VALUES(7576, 'STONE', 'MANAGER', 7839,TO_DATE('3-ABR-1981', 'DD-MON-YYYY'), 2975, NULL, 70);
    INSERT INTO EMP VALUES(7664, 'NAVARRO', 'SALESMAN', 7698,TO_DATE('29-SEP-1981', 'DD-MON-YYYY'), 1250, 1400, 80);
    INSERT INTO EMP VALUES(7618, 'JAKE', 'MANAGER', 7839,TO_DATE('2-MAY-1981', 'DD-MON-YYYY'), 2850, NULL, 80);
    INSERT INTO EMP VALUES(7792, 'CLEIRK', 'MANAGER', 7839,TO_DATE('10-JUN-1981', 'DD-MON-YYYY'), 2450, NULL, 50);
    INSERT INTO EMP VALUES(7798, 'STITS', 'ANALYST', 7566,TO_DATE('8-DIC-1982', 'DD-MON-YYYY'), 3000, NULL, 60);
    INSERT INTO EMP VALUES(7899, 'REPS', 'PRESIDENT', NULL,TO_DATE('1-NOV-1981', 'DD-MON-YYYY'), 5000, NULL, 50);
    INSERT INTO EMP VALUES(7854, 'TURNOR', 'SALESMAN', 7698,TO_DATE('18-SEP-1981', 'DD-MON-YYYY'), 1500, 0, 70);
    INSERT INTO EMP VALUES(7886, 'EVA', 'CLERK', 7788,TO_DATE('1-ENE-1983', 'DD-MON-YYYY'), 1100, NULL, 60);
    INSERT INTO EMP VALUES(7910, 'PETER', 'CLERK', 7698,TO_DATE('13-DIC-1981', 'DD-MON-YYYY'), 950, NULL, 80);
    INSERT INTO EMP VALUES(7912, 'HENRY', 'ANALYST', 7566,TO_DATE('23-DIC-1981', 'DD-MON-YYYY'), 3000, NULL, 60);
    INSERT INTO EMP VALUES(7944, 'MULLER', 'CLERK', 7782,TO_DATE('3-ENE-1982', 'DD-MON-YYYY'), 1300, NULL, 50);

    COMMIT;
    ```

Una vez hemos creado los respectivos usuarios con cada base de datos, procederemos a conectar nuestro oracle1 con oracle2, para ello haremos lo siguiente:

- Compruebo la conectividad de oracle1 a oracle2:

    ```sql
    oracle@oracle:~$ tnsping 192.168.1.19

    TNS Ping Utility for Linux: Version 19.0.0.0.0 - Production on 23-NOV-2022 12:55:11

    Copyright (c) 1997, 2019, Oracle.  All rights reserved.


    Used parameter files:
    /opt/oracle/product/19c/dbhome_1/network/admin/sqlnet.ora

    Used HOSTNAME adapter to resolve the alias
    Attempting to contact (DESCRIPTION=(CONNECT_DATA=(SERVICE_NAME=))(ADDRESS=(PROTOCOL=tcp)(HOST=192.168.1.19)(PORT=1521)))
    OK (10 msec)
    ```

- Como hemos podido comprobar, tenemos conectividad, con lo que procederemos a modificar el fichero de configuración /opt/oracle/product/19c/dbhome_1/network/admin/tnsnames.ora añadiendo la siguiente línea para indicar el servidor de oracle2:

    ```shell
    ORACLE2 =
    (DESCRIPTION =
        (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.1.19)(PORT = 1521))
        (CONNECT_DATA =
        (SERVER = DEDICATED)
        (SERVICE_NAME = ORCLCDB)
        )
    )
    ```

- Ahora, iniciamos sesión con nuestro usuario creado anteriormente y ejecutamos el siguiente comando para establecer la conexión con oracle2:

    ```sql
    oracle@oracle:~$ sqlplus joseju10/joseju10

    SQL*Plus: Release 19.0.0.0.0 - Production on Wed Nov 23 13:39:07 2022
    Version 19.3.0.0.0

    Copyright (c) 1982, 2019, Oracle.  All rights reserved.

    Hora de Ultima Conexion Correcta: Mie Nov 23 2022 12:47:31 +01:00

    Conectado a:
    Oracle Database 19c Enterprise Edition Release 19.0.0.0.0 - Production
    Version 19.3.0.0.0

    SQL> CREATE DATABASE LINK oracle2link
    CONNECT TO joseju20 IDENTIFIED BY joseju20
    USING 'oracle2';

    Enlace con la base de datos creado.
    ```

Ahora conectaremos la base oracle2 con oracle1, para ello volvemos a realizar el mismo procedimiento:

- Comprobamos la conectividad de oracle2 a oracle1:

    ```shell
    oracle@oracle2:~$ tnsping 192.168.1.43

    TNS Ping Utility for Linux: Version 19.0.0.0.0 - Production on 23-NOV-2022 14:12:10

    Copyright (c) 1997, 2019, Oracle.  All rights reserved.

    Used parameter files:
    /opt/oracle/product/19c/dbhome_1/network/admin/sqlnet.ora

    Used HOSTNAME adapter to resolve the alias
    Attempting to contact (DESCRIPTION=(CONNECT_DATA=(SERVICE_NAME=))(ADDRESS=(PROTOCOL=tcp)(HOST=192.168.1.43)(PORT=1521)))
    OK (20 msec)
    ```

- Añadimos esta línea en el fichero tnnames.ora de la máquina oracle2:

    ```shell
    ORACLE1 =
    (DESCRIPTION =
        (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.1.43)(PORT = 1521))
        (CONNECT_DATA =
        (SERVER = DEDICATED)
        (SERVICE_NAME = ORCLCDB)
        )
    )
    ```

- Interconectamos la base de oracle1 con oracle2:

    ```sql
    oracle@oracle2:~$ sqlplus joseju20/joseju20

    SQL*Plus: Release 19.0.0.0.0 - Production on Wed Nov 23 13:49:56 2022
    Version 19.3.0.0.0

    Copyright (c) 1982, 2019, Oracle.  All rights reserved.

    Hora de Ultima Conexion Correcta: Mie Nov 23 2022 13:35:00 +01:00

    Conectado a:
    Oracle Database 19c Enterprise Edition Release 19.0.0.0.0 - Production
    Version 19.3.0.0.0

    SQL> CREATE DATABASE LINK oracle1link
    CONNECT TO joseju10 IDENTIFIED BY joseju10
    USING 'oracle1';

    Enlace con la base de datos creado.
    ```

- Ahora para comprobar que todo funciona, desde oracle1 haremos una consulta a la tabla dept de oracle2:

    ```sql
    SQL> select *
    2  from dept@oracle2link;

    DEPTNO     DNAME	      LOC
    ---------- -------------- -------------
    50         RECOUNTING     MIAMI
    60         SEARCH	      FLORIDA
    70         SHELLS	      NEW JERSEY
    80         OPTIONS	      WASHINTON
    ```

- Realizamos la misma prueba desde oracle2 hasta oracle1:

    ```sql
    SQL> select *    
    2  from dept@oracle1link;

    DEPTNO     DNAME	      LOC
    ---------- -------------- -------------
    10         ACCOUNTING	  NEW YORK
    20         RESEARCH	      DALLAS
    30         SALES	      CHICAGO
    40         OPERATIONS	  BOSTON
    ```

## Interconexión entre dos servidores de datos Postgres.

Lo primero que haremos en este punto es crear dos bases de datos en cada uno de los servidores de postgres y las conectaremos entre sí. Para ello seguiremos los correspondientes pasos:

- Primero creamos la base de datos en postgres1:

    ```sql
    postgres@postgres1:~$ psql
    psql (13.8 (Debian 13.8-0+deb11u1))
    Digite «help» para obtener ayuda.

    postgres=# CREATE DATABASE postgres1;
    CREATE DATABASE
    ```

- Seguidamente, creamos el usuario "postgres1" y le damos privilegios sobre la base de datos creada anteriormente:

    ```sql
    postgres=# CREATE USER postgres1 WITH PASSWORD 'postgres';
    CREATE ROLE
    postgres=# GRANT ALL PRIVILEGES ON DATABASE postgres1 TO postgres1;
    GRANT
    ```

- Ahora procedemos a acceder con el usuario recién creado:

    ```sql
    postgres@postgres1:~$ psql -h localhost -U postgres1 -d postgres1
    Contraseña para usuario postgres1: 
    psql (13.8 (Debian 13.8-0+deb11u1))
    Conexión SSL (protocolo: TLSv1.3, cifrado: TLS_AES_256_GCM_SHA384, bits: 256, compresión: desactivado)
    Digite «help» para obtener ayuda.

    postgres1=>
    ```

- Y creamos la tablas con su correspondiente información:

    ```sql
    postgres1=> CREATE TABLE DEPT
    (DEPTNO NUMERIC(2),
    DNAME VARCHAR(14),
    LOC VARCHAR(13),
    CONSTRAINT PK_DEPT PRIMARY KEY (DEPTNO));
    CREATE TABLE

    postgres1=> postgres1=> CREATE TABLE EMP
    (EMPNO NUMERIC(4),
    ENAME VARCHAR(10),
    JOB VARCHAR(9),
    MGR NUMERIC(4),
    HIREDATE DATE,
    SAL NUMERIC(7, 2),
    COMM NUMERIC(7, 2),
    DEPTNO NUMERIC(2),
    CONSTRAINT FK_DEPTNO FOREIGN KEY (DEPTNO) REFERENCES DEPT (DEPTNO),
    CONSTRAINT PK_EMP PRIMARY KEY (EMPNO));
    CREATE TABLE

    postgres1=> INSERT INTO DEPT VALUES (10, 'ACCOUNTING', 'NEW YORK');
    INSERT INTO DEPT VALUES (20, 'RESEARCH', 'DALLAS');
    INSERT INTO DEPT VALUES (30, 'SALES', 'CHICAGO');
    INSERT INTO DEPT VALUES (40, 'OPERATIONS', 'BOSTON');

    postgres1=> INSERT INTO EMP VALUES(7499, 'ALLEN', 'SALESMAN', 7698,TO_DATE('20-FEB-1981', 'DD-MON-YYYY'), 1600, 300, 30);
    INSERT INTO EMP VALUES(7521, 'WARD', 'SALESMAN', 7698,TO_DATE('22-FEB-1981', 'DD-MON-YYYY'), 1250, 500, 30);
    INSERT INTO EMP VALUES(7654, 'MARTIN', 'SALESMAN', 7698,TO_DATE('28-SEP-1981', 'DD-MON-YYYY'), 1250, 1400, 30);
    INSERT INTO EMP VALUES(7698, 'BLAKE', 'MANAGER', 7839,TO_DATE('1-MAY-1981', 'DD-MON-YYYY'), 2850, NULL, 30);
    INSERT INTO EMP VALUES(7782, 'CLARK', 'MANAGER', 7839,TO_DATE('9-JUN-1981', 'DD-MON-YYYY'), 2450, NULL, 10);
    INSERT INTO EMP VALUES(7839, 'KING', 'PRESIDENT', NULL,TO_DATE('17-NOV-1981', 'DD-MON-YYYY'), 5000, NULL, 10);
    INSERT INTO EMP VALUES(7844, 'TURNER', 'SALESMAN', 7698,TO_DATE('8-SEP-1981', 'DD-MON-YYYY'), 1500, 0, 30);
    ```

Ahora procederemos a hacer lo mismo en postgres2:

- Creamos base de datos:

    ```sql
    postgres@postgres2:~$ psql
    psql (13.8 (Debian 13.8-0+deb11u1))
    Digite «help» para obtener ayuda.

    postgres=# CREATE DATABASE postgres2;
    CREATE DATABASE
    ```

- Creamos el usuario:

    ```sql
    postgres=# CREATE USER postgres2 WITH PASSWORD 'postgres';
    CREATE ROLE
    postgres=# GRANT ALL PRIVILEGES ON DATABASE postgres2 TO postgres2;
    GRANT
    ```

- Accedemos con el usuario recién creado:

    ```sql
    postgres@postgres2:~$ psql -h localhost -U postgres2 -d postgres2
    Contraseña para usuario postgres2: 
    psql (13.8 (Debian 13.8-0+deb11u1))
    Conexión SSL (protocolo: TLSv1.3, cifrado: TLS_AES_256_GCM_SHA384, bits: 256, compresión: desactivado)
    Digite «help» para obtener ayuda.

    postgres2=>
    ```

- Y creamos las tablas con su correspondiente información:

    ```sql
    postgres2=> CREATE TABLE DEPT
    (DEPTNO NUMERIC(2),
    DNAME VARCHAR(14),
    LOC VARCHAR(13),
    CONSTRAINT PK_DEPT PRIMARY KEY (DEPTNO));
    CREATE TABLE

    postgres2=> CREATE TABLE EMP
    (EMPNO NUMERIC(4),
    ENAME VARCHAR(10),
    JOB VARCHAR(9),
    MGR NUMERIC(4),
    HIREDATE DATE,
    SAL NUMERIC(7, 2),
    COMM NUMERIC(7, 2),
    DEPTNO NUMERIC(2),
    CONSTRAINT FK_DEPTNO FOREIGN KEY (DEPTNO) REFERENCES DEPT (DEPTNO),
    CONSTRAINT PK_EMP PRIMARY KEY (EMPNO));
    CREATE TABLE

    postgres2=> INSERT INTO DEPT VALUES (50, 'RECOUNTING', 'MIAMI');
    INSERT INTO DEPT VALUES (60, 'SEARCH', 'FLORIDA');
    INSERT INTO DEPT VALUES (70, 'SHELLS', 'NEW JERSEY');
    INSERT INTO DEPT VALUES (80, 'OPTIONS', 'WASHINTON');

    postgres2=> INSERT INTO EMP VALUES(7419, 'NEIL', 'SALESMAN', 7698,TO_DATE('21-FEB-1981', 'DD-MON-YYYY'), 1600, 300, 80);
    INSERT INTO EMP VALUES(7531, 'RICKY', 'SALESMAN', 7698,TO_DATE('23-FEB-1981', 'DD-MON-YYYY'), 1250, 500, 80);
    INSERT INTO EMP VALUES(7664, 'NAVARRO', 'SALESMAN', 7698,TO_DATE('29-SEP-1981', 'DD-MON-YYYY'), 1250, 1400, 80);
    INSERT INTO EMP VALUES(7618, 'JAKE', 'MANAGER', 7839,TO_DATE('2-MAY-1981', 'DD-MON-YYYY'), 2850, NULL, 80);
    INSERT INTO EMP VALUES(7792, 'CLEIRK', 'MANAGER', 7839,TO_DATE('10-JUN-1981', 'DD-MON-YYYY'), 2450, NULL, 50);
    INSERT INTO EMP VALUES(7899, 'REPS', 'PRESIDENT', NULL,TO_DATE('1-NOV-1981', 'DD-MON-YYYY'), 5000, NULL, 50);
    INSERT INTO EMP VALUES(7854, 'TURNOR', 'SALESMAN', 7698,TO_DATE('18-SEP-1981', 'DD-MON-YYYY'), 1500, 0, 70);
    ```

Ahora que hemos creado las dos bases de datos con sus respetivos usuarios, procederemos a conectarlas entre sí, para ello haremos lo siguiente:

- Dentro del fichero de configuración /etc/postgresql/13/main/postgresql.conf se buscamos la siguiente directiva:

    ```shell
    listen_addresses = 'localhost'          # what IP address(es) to listen on;
    ```

- Modificamos la palabra "localhost" por "*":

    ```shell
    listen_addresses = '*'          # what IP address(es) to listen on;
    ```

- Seguidamente, procedemos a modificar el fichero /etc/postgresql/13/main/pg_hba.conf, con la siguiente información:

    ```shell
    host    replication     all             127.0.0.1/32            md5
    ```

- Modificamos los parámetros por los siguientes:

    ```shell
    host    all     all             all            md5
    ```

- Reiniciamos el servicio de postgres y comprobamos mediante netstat que el puerto de postgres está bien configurado:

    ```shell
    root@postgres1:~# systemctl restart postgresql
    root@postgres1:~# netstat -tln
    Active Internet connections (only servers)
    Proto Recv-Q Send-Q Local Address           Foreign Address         State      
    tcp        0      0 0.0.0.0:5432            0.0.0.0:*               LISTEN
    ```

Seguimos los mismos pasos que hicimos anteriormente en la máquina postgres2 modificando los ficheros postgresql.conf y pg_hba.conf de la misma forma que explicamos anteriormente y comprobamos mediante netstat que todo funciona:

- Comprobamos:

    ```shell
    root@postgres2:~# /var/lib/postgresql# systemctl restart postgresql
    root@postgres2:~# /var/lib/postgresql# netstat -tln
    Active Internet connections (only servers)
    Proto Recv-Q Send-Q Local Address           Foreign Address         State      
    tcp        0      0 0.0.0.0:5432            0.0.0.0:*               LISTEN     
    ```

Ahora que hemos configurado el servicio postgres en ambas máquinas para permitir conexiones remotas, procederemos a interconectar las bases de datos creadas anteriormente entre si. Para ello primero instalamos el siguiente paquete en ambas máquinas:

```shell
root@postgres1:~# apt install postgresql-contrib

root@postgres2:~# apt install postgresql-contrib
```

Posteriormente, configuraremos ambas máquinas para crear un enlace entre ellas:

- Accedemos a la base de datos "postgres1":

    ```sql
    postgres@postgres1:~$ psql -d postgres1
    psql (13.8 (Debian 13.8-0+deb11u1))
    Digite «help» para obtener ayuda.

    postgres1=# 
    ```

- Creamos el siguiente link:

    ```sql
    postgrest1=# CREATE EXTENSION dblink;
    CREATE EXTENSION
    ```

- Realizamos el mismo procedimiento en postgres2:

    ```sql
    postgres@postgres2:~$ psql -d postgres2
    psql (13.8 (Debian 13.8-0+deb11u1))
    Digite «help» para obtener ayuda.

    postgres2=# CREATE EXTENSION dblink;
    CREATE EXTENSION
    ```

Finalmente comprobamos que funciona haciendo una consulta a la tabla dept desde postgres1 a postgres2 y viceversa:

- Select a postgres2:

    ```sql
    postgres1=# SELECT *
    FROM dblink('dbname=postgres2 host=192.168.1.22 user=postgres2 password=postgres','SELECT deptno,dname FROM dept') AS P(deptno NUMERIC(2),dname VARCHAR(14));
    deptno |   dname    
    --------+------------
        50 | RECOUNTING
        60 | SEARCH
        70 | SHELLS
        80 | OPTIONS
    (4 filas)
    ```

- Select a postgres1:

    ```sql
    postgres2=# SELECT *
    postgres2-# FROM dblink('dbname=postgres1 host=192.168.1.23 user=postgres1 password=postgres','SELECT deptno,dname FROM dept') AS P(deptno NUMERIC(2),dname VARCHAR(14));
    deptno |   dname    
    --------+------------
        10 | ACCOUNTING
        20 | RESEARCH
        30 | SALES
        40 | OPERATIONS
    (4 filas)
    ```

## Interconexión entre un servidor Oracle y otro Postgres.

En este punto inteconectararemos la base de datos creada en oracle1 con postgres2, para ello haremos lo siguiente:

- Primero instalamos la paquetería necesaria para configurar los drivers:

    ```shell
    apt install unixodbc odbc-postgres
    ```

A continuación, debemos configurar unixodbc para conectar los drivers, haremos lo siguiente:

- Nos fijamos en la siguiente línea dentro del fichero de configuración /etc/odbcinst.ini:

    ```shell
    [PostgreSQL Unicode]
    ```

- Aquí viene el nombre de los drivers de postgres, con lo que teniendo en cuenta esta información, añadimos la siguiente línea dentro del fichero /etc/odbc.ini:
    
    ```shell
    [PSQLU]
    Debug           = 0
    CommLog         = 0
    ReadOnly        = 0
    Driver          = PostgreSQL
    Servername      = 192.168.1.22
    Username        = postgres2
    Password        = postgres
    Port            = 5432
    Database        = postgres2
    Trace           = 0
    TraceFile       = /tmp/sql.log
    ```

- Comprobamos que hay conectividad:

    ```shell
    root@oracle:~# isql PSQLU
    +---------------------------------------+
    | Connected!                            |
    |                                       |
    | sql-statement                         |
    | help [tablename]                      |
    | quit                                  |
    |                                       |
    +---------------------------------------+
    SQL> select * from dept;
    +-------+---------------+--------------+
    | deptno| dname         | loc          |
    +-------+---------------+--------------+
    | 50    | RECOUNTING    | MIAMI        |
    | 60    | SEARCH        | FLORIDA      |
    | 70    | SHELLS        | NEW JERSEY   |
    | 80    | OPTIONS       | WASHINTON    |
    +-------+---------------+--------------+
    SQLRowCount returns 4
    4 rows fetched
    ```
Ahora que hemos configurado el driver, procederemos a configurar oracle para que utilice el driver, para ello haremos lo siguiente:

- Dentro del directorio /opt/oracle/product/19c/dbhome_1/hs/admin/ creamos el fichero initPSQLU.ora con el siguiente contenido:

    ```shell
    HS_FDS_CONNECT_INFO = PSQLU
    HS_FDS_TRACE_LEVEL = DEBUG
    HS_FDS_SHAREABLE_NAME = /usr/lib64/psqlodbcw.so
    HS_LANGUAGE = AMERICAN_AMERICA.WE8ISO8859P1
    set ODBCINI=/etc/odbc.ini
    ```

- Seguidamente, modificamos el fichero de configuración /opt/oracle/product/19c/dbhome_1/network/admin/listener.ora con el siguiente contenido:

    ```shell
    SID_LIST_LISTENER =
    (SID_LIST =
        (SID_DESC =
        (SID_NAME = PSQLU)
        (ORACLE_HOME = /opt/oracle/product/19c/dbhome_1)
        (PROGRAM = dg4odbc)
        )
    )
    ```

- A continuación, dentro del fichero tnsnames.ora, añadimos lo siguiente:

    ```shell
    PSQLU  =
    (DESCRIPTION=
        (ADDRESS=(PROTOCOL=tcp)(HOST=localhost)(PORT=1521))
        (CONNECT_DATA=(SID=PSQLU))
        (HS=OK)
    )
    ```
- Ahora reiniciamos oracle:

    ```shell
    oracle@oracle:~$ lsnrctl stop

    LSNRCTL for Linux: Version 19.0.0.0.0 - Production on 23-NOV-2022 20:44:24

    Copyright (c) 1991, 2019, Oracle.  All rights reserved.

    Connecting to (DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=0.0.0.0)(PORT=1521)))
    The command completed successfully
    oracle@oracle:~$ lsnrctl start

    LSNRCTL for Linux: Version 19.0.0.0.0 - Production on 23-NOV-2022 20:44:27

    Copyright (c) 1991, 2019, Oracle.  All rights reserved.

    Starting /opt/oracle/product/19c/dbhome_1/bin/tnslsnr: please wait...

    TNSLSNR for Linux: Version 19.0.0.0.0 - Production
    System parameter file is /opt/oracle/product/19c/dbhome_1/network/admin/listener.ora
    Log messages written to /opt/oracle/diag/tnslsnr/oracle/listener/alert/log.xml
    Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=0.0.0.0)(PORT=1521)))
    Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(KEY=EXTPROC1521)))

    Connecting to (DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=0.0.0.0)(PORT=1521)))
    STATUS of the LISTENER
    ------------------------
    Alias                     LISTENER
    Version                   TNSLSNR for Linux: Version 19.0.0.0.0 - Production
    Start Date                23-NOV-2022 20:44:27
    Uptime                    0 days 0 hr. 0 min. 0 sec
    Trace Level               off
    Security                  ON: Local OS Authentication
    SNMP                      OFF
    Listener Parameter File   /opt/oracle/product/19c/dbhome_1/network/admin/listener.ora
    Listener Log File         /opt/oracle/diag/tnslsnr/oracle/listener/alert/log.xml
    Listening Endpoints Summary...
    (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=0.0.0.0)(PORT=1521)))
    (DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(KEY=EXTPROC1521)))
    Services Summary...
    Service "PSQLU" has 1 instance(s).
    Instance "PSQLU", status UNKNOWN, has 1 handler(s) for this service...
    The command completed successfully
    ```

- Nos conectamos a oracle con nuestro usuario:

    ```sql
    oracle@oracle:~$ sqlplus joseju10/joseju10

    SQL*Plus: Release 19.0.0.0.0 - Production on Wed Nov 23 20:45:52 2022
    Version 19.3.0.0.0

    Copyright (c) 1982, 2019, Oracle.  All rights reserved.

    Hora de Ultima Conexion Correcta: Mie Nov 23 2022 13:59:25 +01:00

    Conectado a:
    Oracle Database 19c Enterprise Edition Release 19.0.0.0.0 - Production
    Version 19.3.0.0.0

    SQL> 
    ```
- Creamos un link con la base de datos de postgres:

    ```sql 
    SQL> CREATE DATABASE LINK postgreslink
    2  CONNECT TO "postgres2" IDENTIFIED BY "postgres"
    3  USING 'PSQLU';

    Enlace con la base de datos creado.
    ```
- Hacemos un select a la tabla dept de postgres para comprobar que funciona:

    ```sql
    SQL> select *
    2  from "dept"@postgreslink;

    DEPTNO     DNAME	      LOC
    ---------- -------------- -------------
    50         RECOUNTING     MIAMI
    60         SEARCH	      FLORIDA
    70         SHELLS	      NEW JERSEY
    80         OPTIONS	      WASHINTON
    ```

Ahora que hemos conectado oracle a postgres, lo configuraremos desde postgres a oracle, para ello haremos lo siguiente:

- Primero instalamos los paquetes necesarios:

    ```shell
    postgres@postgres2:~$ apt install libaio1 postgresql-server-dev-all build-essential git
    ```
- Seguidamente descargamos los siguientes archivos:

    ```shell
    postgres@postgres2:~$ wget https://download.oracle.com/otn_software/linux/instantclient/211000/instantclient-basic-linux.x64-21.1.0.0.0.zip

    postgres@postgres2:~$ wget https://download.oracle.com/otn_software/linux/instantclient/211000/instantclient-sdk-linux.x64-21.1.0.0.0.zip

    postgres@postgres2:~$ wget https://download.oracle.com/otn_software/linux/instantclient/211000/instantclient-sqlplus-linux.x64-21.1.0.0.0.zip
    ```
- Descomprimimos los archivos recién descargados:

    ```shell
    postgres@postgres2:~$ unzip instantclient-basic-linux.x64-21.1.0.0.0.zip

    postgres@postgres2:~$ unzip instantclient-sqlplus-linux.x64-21.1.0.0.0.zip 

    postgres@postgres2:~$ unzip instantclient-sdk-linux.x64-21.1.0.0.0.zip
    ```
- Ahora exportaremos las siguientes variables de entorno:

    ```shell
    postgres@postgres2:~$ export ORACLE_HOME=/home/postgres/instantclient_21_1
    postgres@postgres2:~$ export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$ORACLE_HOME
    postgres@postgres2:~$ export PATH=$PATH:$ORACLE_HOME
    ```
- Ahora nos intentamos conectar a nuestro usuario joseju10 de la máquina de oracle:

    ```sql
    postgres@postgres2:~$ sqlplus joseju10/joseju10@192.168.1.43/ORCLCDB

    SQL*Plus: Release 21.0.0.0.0 - Production on Wed Nov 23 21:12:23 2022
    Version 21.1.0.0.0

    Copyright (c) 1982, 2020, Oracle.  All rights reserved.

    Hora de Ultima Conexion Correcta: Mie Nov 23 2022 20:45:52 +01:00

    Conectado a:
    Oracle Database 19c Enterprise Edition Release 19.0.0.0.0 - Production
    Version 19.3.0.0.0

    SQL>
    ```
Una vez hemos comprobado que el cliente oracle funciona correctamente, procederemos a la compliación de oracle_fwd, para ello haremos lo siguiente:

- Clonamos el siguiente repositorio:

    ```shell
    postgres@postgres2:~$ git clone https://github.com/laurenz/oracle_fdw.git
    Clonando en 'oracle_fdw'...
    remote: Enumerating objects: 2489, done.
    remote: Counting objects: 100% (343/343), done.
    remote: Compressing objects: 100% (124/124), done.
    remote: Total 2489 (delta 234), reused 324 (delta 219), pack-reused 2146
    Recibiendo objetos: 100% (2489/2489), 1.43 MiB | 1.13 MiB/s, listo.
    Resolviendo deltas: 100% (1708/1708), listo.
    ```
- Accemos al directorio clonado y compilamos:

    ```shell
    postgres@postgres2:~/oracle_fdw$ make
    gcc -Wall -Wmissing-prototypes -Wpointer-arith -Wdeclaration-after-statement -Werror=vla -Wendif-labels -Wmissing-format-attribute -Wimplicit-fallthrough=3 -Wformat-security -fno-strict-aliasing -fwrapv -fexcess-precision=standard -Wno-format-truncation -Wno-stringop-truncation -g -g -O2 -fstack-protector-strong -Wformat -Werror=format-security -fno-omit-frame-pointer -fPIC -I"/home/postgres/instantclient_21_1/sdk/include" -I"/home/postgres/instantclient_21_1/oci/include" -I"/home/postgres/instantclient_21_1/rdbms/public" -I"/home/postgres/instantclient_21_1/"  -I. -I./ -I/usr/include/postgresql/13/server -I/usr/include/

    root@postgres2:/home/postgres/oracle_fdw# make install
    /bin/mkdir -p '/usr/lib/postgresql/13/lib'
    /bin/mkdir -p '/usr/share/postgresql/13/extension'
    /bin/mkdir -p '/usr/share/postgresql/13/extension'
    /bin/mkdir -p '/usr/share/doc/postgresql-doc-13/extension'
    /usr/bin/install -c -m 755  oracle_fdw.so '/usr/lib/postgresql/13/lib/oracle_fdw.so'
    /usr/bin/install -c -m 644 .//oracle_fdw.control '/usr/share/postgresql/13/extension/'
    /usr/bin/install -c -m 644 .//oracle_fdw--1.2.sql .//oracle_fdw--1.0--1.1.sql .//oracle_fdw--1.1--1.2.sql  '/usr/share/postgresql/13/extension/'
    /usr/bin/install -c -m 644 .//README.oracle_fdw '/usr/share/doc/postgresql-doc-13/extension/'
    ```

- Seguidamente, indicamos en el fichero de configuración oracle.conf el siguiente contenido para indicarle las librerías de Oracle:

    ```shell
    root@postgres2:/home/postgres/oracle_fdw# echo '/home/postgres/instantclient\_21\_1' | tee /etc/ld.so.conf.d/oracle.conf
    ```

En este punto ya tenemos configurado el apartado de datos con respecto a la configuración de postgres para acceder a oracle, lo que haremos será crear un esquema con el usuario por defecto de postgres copiando la base de datos que tenemos en oracle a un esquema que crearemos en este punto. 

A dicho esquema le otorgaremos los correspondientes permisos para que el usuario postgres2 pueda crear tablas, realizar consultas... Para ello seguiremos los siguientes pasos:

-  Creamos el enlace de la base de datos de oracle1 a posgres2, para ello accedemos a la base de datos postgres2:

    ```shell
    postgres@postgres2:~/oracle_fdw$ psql -d postgres2
    psql (13.8 (Debian 13.8-0+deb11u1))
    Digite «help» para obtener ayuda.

    postgres2=>
    ```
- Creamos dicho enlace:

    ```sql
    postgres2=# CREATE EXTENSION oracle_fdw;
    CREATE EXTENSION
    ```
- Generamos un esquema de nombre oracle:

    ```sql
    postgres2=# CREATE SCHEMA oracle;
    CREATE SCHEMA
    ```
- Seguidamente, mapeamos el usuario local al usuario remoto de oracle:

    ```sql
    postgres2=# CREATE USER MAPPING FOR oracle SERVER oracle OPTIONS (user 'joseju10', password 'joseju10');
    CREATE USER MAPPING
    ```
- Otorgamos permisos al usuario local postgres2:

    ```sql
    postgres2=# GRANT ALL PRIVILEGES ON SCHEMA oracle TO postgres2;
    GRANT
    ```
    ```sql
    postgres2=# GRANT ALL PRIVILEGES ON FOREIGN SERVER oracle TO postgres2;
    GRANT
    ```
- Iniciamos sesión con el usuario postgres2:

    ```sql
    postgres2@postgres2:~$ psql -h localhost -U postgres2 -d postgres2
    Contraseña para usuario postgres2: 
    psql (13.8 (Debian 13.8-0+deb11u1))
    Conexión SSL (protocolo: TLSv1.3, cifrado: TLS_AES_256_GCM_SHA384, bits: 256, compresión: desactivado)
    Digite «help» para obtener ayuda.

    postgres2=>
    ```
- Importamos el esquema creado anteriormente:

    ```sql
    prueba2=# IMPORT FOREIGN SCHEMA "joseju10" FROM SERVER oracle INTO oracle;
    IMPORT FOREIGN SCHEMA
    ```
- Finalmente realizamos una consulta a la tabla dept del servidor para comprobar si funciona:

    ```sql
    postgres1=> select *
    postgres1-> from dept;
    deptno |   dname    |   loc    
    --------+------------+----------
        10 | ACCOUNTING | NEW YORK
        20 | RESEARCH   | DALLAS
        30 | SALES      | CHICAGO
        40 | OPERATIONS | BOSTON
    (4 filas)
    ```